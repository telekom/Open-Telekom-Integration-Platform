<!--
SPDX-FileCopyrightText: 2026 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# AI Gateway Installation on AWS EKS

This guide describes how to deploy the **AI Gateway** alongside an existing
[Gateway and Iris installation](installation_on_eks.md). The AI Gateway is identical to the regular Gateway — it
uses the same Helm chart, the same images, and the same configuration structure. The key differences are:

- The AI Gateway is deployed as a separate Helm release with its own ingress hostname (`ai-gateway.<your-hostname>`),
  its own database, and its own issuer key pair secret.
- Iris is **shared** between the Gateway and the AI Gateway. No additional Iris deployment or Iris realm
  configuration is required.

## Prerequisites

Complete the [Installation on AWS EKS](installation_on_eks.md) guide first. The following must already be in place:

- EKS cluster with ingress class and NLB/ALB setup
- Identity-Iris deployed and configured (including the `rover` realm and client)
- Aurora PostgreSQL RDS database

## Database Setup

The AI Gateway requires its own database and user. Connect to the RDS Query Editor and execute the following
against the `postgres` database:

```sql
CREATE DATABASE ai_gateway;
CREATE USER ai_gateway WITH PASSWORD '<change-me>';
GRANT ALL PRIVILEGES ON DATABASE ai_gateway TO ai_gateway;
```

Then switch the Query Editor to the `ai_gateway` database and execute:

```sql
GRANT ALL ON SCHEMA public TO ai_gateway;
```

## Deploy the AI Gateway

### Admin API Key

Generate a `.htpasswd` entry for the AI Gateway admin API. Replace `aiGatewayAdminApiKey` with your desired password:

```shell
htpasswd -b -n admin aiGatewayAdminApiKey
```

### Issuer Service Key Pair

Create a dedicated TLS key pair secret for the AI Gateway. This is independent of the regular Gateway's secret:

```shell
# Create cert and key pair
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ai-tls.key -out ai-tls.crt -subj "/CN=ai-gateway.<your-hostname>"
# Create base64 encoded sha1sum for usage as kid
AI_TLS_KEY_KID=$(sha1sum ai-tls.crt | awk '{printf "%s", $1}' | base64)
kubectl create secret tls ai-gateway-tls-rotated --cert=ai-tls.crt --key=ai-tls.key --dry-run=client -o go-template='
apiVersion: v1
kind: Secret
metadata:
  namespace: o28m
  name: {{ .metadata.name }}
type: kubernetes.io/tls
data:
  tls.key: {{ index .data "tls.key" }}
  tls.kid: changeme
  tls.crt: {{ index .data "tls.crt" }}
  prev-tls.key: {{ index .data "tls.key" }}
  prev-tls.kid: changeme
  prev-tls.crt: {{ index .data "tls.crt" }}
  next-tls.key: {{ index .data "tls.key" }}
  next-tls.kid: changeme
  next-tls.crt: {{ index .data "tls.crt" }}
' | sed "s/changeme/${AI_TLS_KEY_KID}/g" | kubectl apply -f -
```

### Configure and Deploy the AI Gateway

1. Create an `ai-gateway.values.local.yaml` file with the following content:
    ```yaml

    global:
      zone: <zone>  # The zone in which the application is deployed. This is determined by the central configuration management.

      # General ingress settings
      ingress:
        ingressClassName: alb-public  # as set in the IngressClass
        annotations:
          alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS":443}]'
          alb.ingress.kubernetes.io/security-groups: sg-{ALBSecurityGroup}, sg-{ClusterSecurityGroup}
          alb.ingress.kubernetes.io/tags: example=tag,another=one

      # Database config
      database:
        location: external
        username: ai_gateway # as set in the RDS database
        database: ai_gateway # as set in the RDS database
        password: <change-me> # as set in the RDS database

    externalDatabase:
      host: {RDS_Write_Endpoint}.{Region}.rds.amazonaws.com # as provided by the RDS database
      ssl: false

    postgresql:
      image: <registry>/o28m/postgresql

    migrations: bootstrap # Initial setup of the database and Kong. Change to "upgrade" ONLY for version upgrades.

    image: <registry>/o28m/gateway-kong-postgresql-fix:latest # The kong image

    jumper:
      image: <registry>/o28m/gateway-jumper
      existingJwkSecretName: ai-gateway-tls-rotated
      stargateUrl: https://ai-gateway.<your-hostname> # The AI Gateway URL
      issuerUrl: https://ai-gateway.<your-hostname>/auth/realms # This URL points to the AI Gateway's issuer-service.

    job:
      image: <registry>/o28m/bash-curl

    proxy:
      ingress:
        hosts:
          - host: ai-gateway.<your-hostname> # The AI Gateway proxy hostname
            paths:
              - path: /
                pathType: Prefix

    adminApi:
      ingress:
        hosts:
          - host: ai-gateway-admin.<your-hostname> # The AI Gateway admin API hostname
            paths:
              - path: /
                pathType: Prefix
      gatewayAdminApiKey: aiGatewayAdminApiKey # Use the password specified during the htpasswd command above.
      htpasswd: "admin:$apr1$iXCoWLYQ$oCrteDqJuXOu.SjUfC9MI0" # Use the output of the htpasswd command above.

    issuerService:
      image: <registry>/o28m/gateway-issuer-service
      existingJwkSecretName: ai-gateway-tls-rotated

    # Iris is shared — the allowedIss points to the same Iris rover realm as the regular Gateway.
    plugins:
      enabled: []
      jwtKeycloak:
        enabled: true
        allowedIss:
          - "https://iris.<your-hostname>/auth/realms/rover" # The shared Iris issuer URL.
      requestSizeLimiting:
        enabled: true
      prometheus:
        enabled: false
      zipkin:
        enabled: false

    replicas: 2
    ```

2. Deploy the Helm chart as a separate release:
    ```bash
    namespace=o28m
    helm upgrade -i -n $namespace --create-namespace -f ai-gateway.values.local.yaml ai-gateway gateway-kong-charts/
    ```

3. Register the AI Gateway ingress hostname in DNS. Add a CNAME record for `ai-gateway.<your-hostname>` (and
   `ai-gateway-admin.<your-hostname>`) pointing to the NLB DNS name, the same way as for the regular Gateway.

4. Register the new ALBs created for the AI Gateway in the NLB target group, following the same steps as for the
   regular Gateway.

5. Validate the setup by accessing the AI Gateway admin API using the shared rover client:
    ```bash
    TOKEN=$(curl -X POST https://iris.<your-hostname>/auth/realms/rover/protocol/openid-connect/token \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'client_id=rover' \
    -d 'client_secret=<changeme>' \
    -d 'grant_type=client_credentials' | jq -r '.access_token'
    )
    curl -v -H "Authorization: Bearer ${TOKEN}" https://ai-gateway-admin.<your-hostname>/admin-api/consumers
    ```

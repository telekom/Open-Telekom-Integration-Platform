<!--
SPDX-FileCopyrightText: 2025 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Open Telekom Integration Platform on minikube

[![GitHub Release](https://img.shields.io/github/v/release/kubernetes/minikube?filter=v1.32.0&display_name=release&label=Minikube)](https://github.com/kubernetes/minikube/releases/tag/v1.32.0)
[![GitHub Release](https://img.shields.io/github/v/release/helm/helm?filter=Helm%20v3.14.0&display_name=release&label=Helm)](https://github.com/helm/helm/releases/tag/v3.14.0)

This guide describes how to install the Open Telekom Integration Platform on minikube. It is intended for development
and testing purposes only.
If you do not know what minikube is, please refer to the [minikube documentation](https://minikube.sigs.k8s.io/docs/).

This guide is written for and tested with minikube version v1.32.0, kubernetes version v1.28.3, and Helm version v3.14.0
on macOS arm64 with Docker.

**Please note that there is currently a bug with Docker v25 preventing macOS environments to successfully emulate
x86-environments. Please consider using Docker v24.**

## Install minikube

Follow the instructions in the [minikube documentation](https://minikube.sigs.k8s.io/docs/start/). Make sure to install
the version displayed on the badge above.

You may use the following command to install minikube using [homebrew](https://brew.sh/) on macOS:

```shell
brew install minikube
```

Start minikube using the following command:

```shell
minikube start --driver=docker --kubernetes-version=v1.28.3
```

## Enable minikube addons

The following minikube addons are required for the Open Telekom Integration Platform:

- ingress and ingress-dns:
  ```shell
  minikube addons enable ingress
  minikube addons enable ingress-dns
  ```

- Please also follow the instructions in
  the [minikube documentation](https://minikube.sigs.k8s.io/docs/handbook/addons/ingress-dns/)
  to configure your local DNS resolver to use the minikube cluster DNS. This is required to access the deployed
  components.  
  Add a block for your local domain (replace the placeholder with the result of `minikube ip`):
    ```shell
  minikube ip
  ```
  ```shell
  kubectl edit configmap coredns -n kube-system
  ```
  ```text
  test:53 {
            errors
            cache 30
            forward . <minikube ip>
    }
  ```

- Also, you may add the following entries to your `/etc/hosts` file:
  ```shell
  127.0.0.1 iris.test
  127.0.0.1 gateway.test
  127.0.0.1 gateway-admin.test
  ```

- storage-provisioner:
  ```shell
  minikube addons enable storage-provisioner
  ```

## Build the images

### Iris

First, clone the [`telekom/identity-iris-keycloak-image`](https://github.com/telekom/identity-iris-keycloak-image)
repository:

```shell
git clone https://github.com/telekom/identity-iris-keycloak-image.git
```

Then, build the image:

```shell
cd identity-iris-keycloak-image
docker build --platform linux/amd64 -t iris -f Dockerfile.multi-stage .
```

### Gateway

#### Gateway-Jumper

Clone the [`telekom/gateway-jumper`](https://github.com/telekom/gateway-jumper) repository:

```shell
git clone https://github.com/telekom/gateway-jumper.git
```

Then, build the image:

```shell
cd gateway-jumper
docker build --platform linux/amd64 -t gateway-jumper -f Dockerfile.multi-stage .
```

#### Gateway-Issuer-Service-Go

Clone the [`telekom/gateway-issuer-service-go`](https://github.com/telekom/gateway-issuer-service-go) repository:

```shell
git clone https://github.com/telekom/gateway-issuer-service-go.git
```

Then, build the image:

```shell
cd gateway-issuer-service-go
docker build --platform linux/amd64 -t gateway-issuer-service -f Dockerfile.multi-stage .
```

**Attention**: The gateway chart will later require a TLS secret in a specific format for the gateway-issuer-service-go and the gateway-jumper to provide everything for oauth token issuing as well as graceful key rotation. Please refer to
the [gateway-rotator](https://github.com/telekom/gateway-rotator?tab=readme-ov-file#architecture)
repository for documentation how the secret format has to be in order to get the whole gateway running.

#### Gateway-Kong

In order to work properly, the gateway chart requires a custom Kong image. More concrete, Kong needs the [`jwt-keycloak`](https://github.com/telekom/kong-plugin-jwt-keycloak)
plugin. The plugin is not part of the official Kong distribution. Additionally, some existing plugins have been patched
to cater for the Open Telekom Integration Platform's needs. The repository containing all of those pieces is called [`telekom/gateway-kong-image`](https://github.com/telekom/gateway-kong-image).

First, clone
the [`telekom/gateway-kong-image`](https://github.com/telekom/gateway-kong-image) repository:

```shell
git clone --recursive https://github.com/telekom/gateway-kong-image.git
```

Then, build the image. Pay attention to the `KONG_VERSION` and `PLUGIN_VERSION` build arguments:

```shell
docker build \
  --platform linux/amd64 \
  -t gateway-kong .
```

#### Bash-Curl

The gateway chart requires a job image with bash and curl installed. Feel free to use any image that fulfills these
requirements. For this guide, a simple image is created:

```Dockerfile
# Dockerfile.bash-curl
FROM quay.io/curl/curl-base:latest

RUN apk add --no-cache bash
```

```shell
docker build --platform linux/amd64 -t bash-curl -f Dockerfile.bash-curl .
```

## Load the images into minikube

Load the images into minikube using the following commands:

```shell
minikube image load iris:latest
minikube image load gateway-jumper:latest
minikube image load gateway-issuer-service:latest
minikube image load gateway-kong:latest
minikube image load bash-curl:latest
```

## Prepare the Helm charts

### Install Helm

Follow the instructions in the [Helm documentation](https://helm.sh/docs/intro/install/) to install Helm. Make sure to
install the version displayed on the badge above.

You may use the following command to install Helm using [homebrew](https://brew.sh/) on macOS:

```shell
brew install helm
```

### Iris

First, clone the [`telekom/identity-iris-keycloak-charts`](https://github.com/telekom/identity-iris-keycloak-charts)
repository:

```shell
git clone https://github.com/telekom/identity-iris-keycloak-charts.git
```

Then, update the values in the Helm chart according to your environment. Pay special attention to the ingresses, domain
names, database connections and the images.

```shell
cd identity-iris-keycloak-charts
```

For this guide, a new file `values.local.yaml` is created with the following content:

```yaml
# values.local.yaml
imagePullPolicy: Never # Required for minikube to use the local images.

keycloak:
  image: iris:latest # Use the previously built image.

ingress:
  hostname: iris.test # Use the hostname specified during ingress-dns setup.
```

### Gateway

First, clone the [`telekom/gateway-kong-charts`](https://github.com/telekom/gateway-kong-charts) repository:

```shell
git clone https://github.com/telekom/gateway-kong-charts.git
```

Then, update the values in the Helm chart according to your environment. Pay special attention to the ingresses, domain
names, database connections and the images.

```shell
cd gateway-kong-charts
```

**Attention**: The gateway chart requires a .htpasswd file for an initial basic authentication. You may use the
following command to create the content of file which later will be stored in a Kubernetes secret. Replace `adminApiKey`
with your desired password. The output of the command will be used in the `values.local.yaml` file.

```shell
htpasswd -b -n admin adminApiKey
```

For this guide, a new file `values.local.yaml` is created with the following content:

```yaml
# values.local.yaml

global:
  storageClassName: standard # Use the storage class of your minikube setup.
  imagePullPolicy: Never # Required for minikube to use the local images.
  database:
    password: postgres

postgresql:
  image: bitnami/postgresql:13 # Or any other PostgreSQL image.
  adminPassword: admin

migrations: bootstrap # Initial setup of the database and Kong. Change to "upgrade" ONLY for version upgrades.

jumper:
  image: gateway-jumper:latest # Use the previously built image.
  imagePullPolicy: Never # Required for minikube to use the local images.
  # When providing an existing secret, it has to compatible with a gateway-rotator managed one
  # Format is specified here https://github.com/telekom/gateway-rotator?tab=readme-ov-file#key-rotation-process
  # For easy startup create a tls.key and tls.crt as well as a random kid for it and duplicate this for the next and prev representations
  existingJwkSecretName: gateway-jwk-rotated

image: gateway-kong:latest # Use the previously built image.
imagePullPolicy: Never # Required for minikube to use the local images.

proxy:
  ingress:
    hosts:
      - host: gateway.test # Use the hostname specified during ingress-dns setup.
        paths:
          - path: /
            pathType: Prefix

job:
  image: bash-curl:latest # Use the previously built image or any image with bash and curl installed.

adminApi:
  htpasswd: "admin:$apr1$iQDtnHAi$iLe6Rgymvoz5avgyij7NC1" # Use the output of the htpasswd command above.
  gatewayAdminApiKey: adminApiKey # Use the password specified during the htpasswd command above.
  ingress:
    hosts:
      - host: gateway-admin.test # Use the hostname specified during ingress-dns setup.
        paths:
          - path: /
            pathType: Prefix

issuerService:
  image: gateway-issuer-service:latest # Use the previously built image.
  imagePullPolicy: Never # Required for minikube to use the local images.
  # When providing an existing secret, it has to compatible with a gateway-rotator managed one
  # Format is specified here https://github.com/telekom/gateway-rotator?tab=readme-ov-file#key-rotation-process
  # For easy startup create a tls.key and tls.crt as well as a random kid for it and duplicate this for the next and prev representations
  existingJwkSecretName: gateway-jwk-rotated

plugins: # Enable only the minimal required plugins for testing purposes.
  prometheus:
    enabled: false # No Prometheus setup for minikube.
  zipkin:
    enabled: false # No Zipkin setup for minikube.
  requestSizeLimiting:
    enabled: true # Enable request size limiting. This limits the request size to 4MB by default.
  jwtKeycloak:
    enabled: true # Enable the JWT Keycloak plugin.
    allowedIss:
      - "https://iris.test/auth/realms/rover" # The previously configured issuer URL of the identity provider to be used for the admin api access.
```

## Install the Helm charts

First, create a new namespace:

```shell
kubectl create namespace platform
```

Then, install the Iris Helm charts:

```shell
cd identity-iris-keycloak-charts
helm upgrade -i -n platform -f values.yaml -f values.local.yaml iris .
```

`helm upgrade -i` will install the chart if it does not exist, or upgrade it if it already exists.

Then, install the Gateway Helm charts:

```shell
cd gateway-kong-charts
helm upgrade -i -n platform -f values.yaml -f values.local.yaml gateway .
```

## Configure the deployed components

In order to use the deployed components, you need to configure them. This guide will show you how to configure the
identity provider and the gateway. Please note that this is only a minimal configuration for testing purposes only.

To access the deployed components from your browser, you need to run the following command in a separate terminal:

```shell
minikube tunnel
```

### Configure the identity provider

1. Retrieve the admin password:
    ```shell
    kubectl get secret -n platform iris -o jsonpath="{.data.adminPassword}" | base64 --decode
    ```

2. Open the Keycloak admin console in your browser: https://iris.test/auth/admin/. Since it is a self-signed
   certificate for local testing purposes, you need to accept the certificate warning.

3. Login with the username `admin` and the password retrieved above.

4. Create a new realm named `default`. Keep the remaining settings as-is.

5. Create a new client-scope named `open-telekom-integration-platform` with the type `optional`.

6. Create a second new client-scope named `client-origin` with the type `default`.

7. Add a new mapper by configuration to the `client-origin` client-scope with the following settings:
    - Mapper Type: `Hardcoded claim`
    - Name: `originZone`
    - Token Claim Name: `originZone`
    - Claim value: `minikube`
    - Claim JSON Type: `String`
    - Add to ID token: `On`
    - Add to access token: `On`
    - Add to userinfo: `On`
    - Add to access token response: `Off`

8. Add a second new mapper by configuration to the `client-origin` client-scope with the following settings:
    - Mapper Type: `Hardcoded claim`
    - Name: `originGateway`
    - Token Claim Name: `originGateway`
    - Claim value: `https://gateway.test`
    - Claim JSON Type: `String`
    - Add to ID token: `On`
    - Add to access token: `On`
    - Add to userinfo: `On`
    - Add to access token response: `Off`

9. Create a new realm called `rover`. Keep the remaining settings as-is.

10. Create a client named `rover` in the realm `rover` and a client named `demo-user` in the realm `default`. You may use the import client feature to import the client
   configuration below:
    <details>

   <summary>Client Configuration</summary>

    ```json
    {
      "clientId": "<rover/demo-user>",
      "name": "<rover/demo-user>",
      "surrogateAuthRequired": false,
      "enabled": true,
      "alwaysDisplayInConsole": false,
      "clientAuthenticatorType": "client-secret",
      "secret": "<changeme>",
      "redirectUris": [],
      "webOrigins": [],
      "notBefore": 0,
      "bearerOnly": false,
      "consentRequired": false,
      "standardFlowEnabled": false,
      "implicitFlowEnabled": false,
      "directAccessGrantsEnabled": false,
      "serviceAccountsEnabled": true,
      "publicClient": false,
      "frontchannelLogout": false,
      "protocol": "openid-connect",
      "attributes": {},
      "authenticationFlowBindingOverrides": {},
      "fullScopeAllowed": false,
      "nodeReRegistrationTimeout": -1,
      "protocolMappers":
      [
        {
          "name": "Client Host",
          "protocol": "openid-connect",
          "protocolMapper": "oidc-usersessionmodel-note-mapper",
          "consentRequired": false,
          "config": {
            "user.session.note": "clientHost",
            "id.token.claim": "true",
            "access.token.claim": "true",
            "claim.name": "clientHost",
            "jsonType.label": "String"
          }
        },
        {
          "name": "Client IP Address",
          "protocol": "openid-connect",
          "protocolMapper": "oidc-usersessionmodel-note-mapper",
          "consentRequired": false,
          "config": {
            "user.session.note": "clientAddress",
            "id.token.claim": "true",
            "access.token.claim": "true",
            "claim.name": "clientAddress",
            "jsonType.label": "String"
          }
        },
        {
          "name": "Client ID",
          "protocol": "openid-connect",
          "protocolMapper": "oidc-usersessionmodel-note-mapper",
          "consentRequired": false,
          "config": {
            "user.session.note": "clientId",
            "id.token.claim": "true",
            "access.token.claim": "true",
            "claim.name": "clientId",
            "jsonType.label": "String"
          }
        }
      ],
      "defaultClientScopes": [
        "web-origins",
        "client-origin",
        "profile",
        "roles",
        "email"
      ],
      "optionalClientScopes": [
        "address",
        "phone",
        "open-telekom-integration-platform",
        "offline_access",
        "microprofile-jwt"
      ],
      "access": {
        "view": true,
        "configure": true,
        "manage": true
      }
    }
    ```
   </details>

## Try it out

You may now try out the deployed components. The following steps will show you how to access the Kong admin API using
the previously configured `rover` client.

1. Retrieve the `rover` client secret from the `rover` client configuration in the Keycloak admin console.
2. Call the Iris token endpoint to retrieve an access token:
    ```shell
    curl -k -X POST "https://iris.test/auth/realms/rover/protocol/openid-connect/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -d "client_id=rover" \
      -d "client_secret=<rover-client-secret>" \
      -d "grant_type=client_credentials"
    ```
3. Use the access token to call the Kong admin API. Replace `<access token>` with the access token retrieved in the
   previous step:
    ```shell
    curl -k -X GET -H "Authorization: Bearer <access token>" \
      https://gateway-admin.test/admin-api/consumers
    ```
   This should return a list of consumers with one entry for the `rover` client.

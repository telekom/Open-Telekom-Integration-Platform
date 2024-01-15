<!--
SPDX-FileCopyrightText: 2023 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Getting Started

## Prerequisites

### Kubernetes

The Open Telekom Integration Platform is designed to be deployed on Kubernetes.
Currently, it is tested with Kubernetes version **1.24**.

#### Additional required Kubernetes features:

- Kubernetes Ingress Controller (preferably [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/))
- Kubernetes DNS (preferably [CoreDNS](https://coredns.io/))
- Persistent Volumes (NFS or [block storage](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html) like gp2)
- TLS certificate management (e.g. [cert-manager](https://cert-manager.io/))

### Databases

The Open Telekom Integration Platform requires the following databases:

- PostgreSQL (version 13) for the API Management component
- PostgreSQL (version 13) for the Identity Management component

**Note:** The Open Telekom Integration Platform is tested with AWS
RDS [Aurora PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html) (version
13.10).
Other versions might work, but are not tested.

## Preparing the images

Follow the instructions in the respective repositories of the products you want to deploy. The image repositories end
with `-image`.
Make sure to make the images available to your Kubernetes cluster.

## Preparing the Helm charts

The Open Telekom Integration Platform's products are preferably deployed using Helm charts.
The Helm charts are available in the respective repositories of the products you want to deploy. The chart repositories
end with `-chart`.
Make sure to update the values in the Helm charts according to your environment.
Pay special attention to the ingresses, domain names, database connections and the images.

It is also possible to use ArgoCD to deploy the Helm charts. Make sure to create the respective application definitions
in ArgoCD.

## Apply the Helm charts

You may apply the Helm charts (using ArgoCD, CICD pipelines or manually) in any order you like. However, the following
order is recommended due to the fact that many products rely on the identity provider and API management:

1. Identity Provider
2. API Management
3. Event-Driven Integration / Pubsub
4. File Transfer

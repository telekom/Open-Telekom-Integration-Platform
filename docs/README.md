<!--
SPDX-FileCopyrightText: 2025 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Open Telekom Integration Platform Documentation

This documentation provides an overview of the Open Telekom Integration Platform, its components, and how to get started
with deploying it.

## Prerequisites

### Kubernetes

The Open Telekom Integration Platform is designed to be deployed on Kubernetes.
Currently, it is tested with Kubernetes version **1.31**.

### Additional required Kubernetes features:

- Kubernetes Ingress Controller (preferably [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/))
- Kubernetes DNS (preferably [CoreDNS](https://coredns.io/))
- Persistent Volumes (NFS or [block storage](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html) like gp2)
- TLS certificate management (e.g. [cert-manager](https://cert-manager.io/))

### Databases

The Open Telekom Integration Platform requires the following databases:

- PostgreSQL (version 14+) for the Gateway component
- PostgreSQL (version 14+) for the Identity Provider component

**Note:** The Open Telekom Integration Platform is tested with AWS
RDS [Aurora PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html) (version
16). Other versions might work, but are not tested.
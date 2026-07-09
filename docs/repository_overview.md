<!--
SPDX-FileCopyrightText: 2025 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Available Repositories

The list below provides an overview of the available repositories of the Open Telekom Integration Platform and its
components.

Every repository is linked to its GitHub page, where you can find more information about the specific component.

## Documentations

- [Overarching documentation](https://github.com/telekom/Open-Telekom-Integration-Platform)
- [Event-Driven Integration / Pubsub](https://github.com/telekom/pubsub-horizon)

## Components and Recommended Versions

The table below provides insights into the generally expected maturity of the existing release versions of the
individual Helm charts and components. It distinguishes between **Latest**, **Staging**, and **Production Ready**.

**Latest** refers to the newest release version of a component that has undergone all regular (integration) tests and
quality checks but for which little operational experience exists. Therefore no direct recommendation can be made for
use in staging (non-prod) environments.
However, you might want to try these versions non-productively in advance to possibly test a new feature as early
adopter.

**Staging** refers to a new release for which operational experience in non-productive (staging) environments already
exists. These versions are candidates for possible productive use.

**Production Ready** refers to a release version for which operational experience in productive environments already
exists and can therefore be recommended for productive use.

### Gateway

| Name                                                                      |                                Latest                                 |                                   Staging                                   |                              Production Ready                               |
|---------------------------------------------------------------------------|:---------------------------------------------------------------------:|:---------------------------------------------------------------------------:|:---------------------------------------------------------------------------:|
| **[Gateway-Kong-Charts](https://github.com/telekom/gateway-kong-charts)** | **[latest](https://github.com/telekom/gateway-kong-charts/releases)** | **[9.9.2](https://github.com/telekom/gateway-kong-charts/releases/9.9.2)** | **[9.6.1](https://github.com/telekom/gateway-kong-charts/releases/9.6.1)** |
| [Gateway-Rotator](https://github.com/telekom/gateway-rotator)             |     [latest](https://github.com/telekom/gateway-rotator/releases)     |     [1.0.0](https://github.com/telekom/gateway-rotator/releases/v1.0.0)     |     [1.0.0](https://github.com/telekom/gateway-rotator/releases/v1.0.0)     |

> **Notes:**
> - [Gateway-Issuer-Service](https://github.com/telekom/gateway-issuer-service) is deprecated and was used for chart versions < 7.0.0. The new Go implementation ([Gateway-Issuer-Service-Go](https://github.com/telekom/gateway-issuer-service-go)) is the successor and will be properly referenced in newer Helm charts.
> - [Gateway-Rotator](https://github.com/telekom/gateway-rotator) is mandatory for gateway helm chart versions > 7.0.0 and is deployed separately using kustomize.

### Identity Provider

| Name                                                                                          |                                     Latest                                      |                                       Staging                                        |                                   Production Ready                                   |
|-----------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:|
| **[Identity-Iris-Keycloak-Charts](https://github.com/telekom/identity-iris-keycloak-charts)** | **[latest](https://github.com/telekom/identity-iris-keycloak-charts/releases)** | **[3.0.0](https://github.com/telekom/identity-iris-keycloak-charts/releases/3.0.0)** | **[3.0.0](https://github.com/telekom/identity-iris-keycloak-charts/releases/3.0.0)** |

### Event-Driven Integration / Pubsub

| Name                                                                                    |                                    Latest                                    | Staging  | Production Ready |
|:----------------------------------------------------------------------------------------|:----------------------------------------------------------------------------:|:--------:|:----------------:|
| **[Pubsub-Horizon-Helm-Charts](https://github.com/telekom/pubsub-horizon-helm-charts)** | **[latest](https://github.com/telekom/pubsub-horizon-helm-charts/releases)** | **tbd.** |     **tbd.**     |

> **Note:** [Pubsub-Horizon-Polaris](https://github.com/telekom/pubsub-horizon-polaris) is deprecated and will be replaced by [Pubsub-Horizon-Golaris](https://github.com/telekom/pubsub-horizon-golaris). However, the horizon-all Helm chart still references Polaris and the Golaris Helm chart is not part of the Horizon Helm charts repository yet. Also, existing Horizon deployment guides still rely on old components/component versions. This will be revised and reworked in the future.

## Additional components

### Gateway

- [Kong-Plugin-JWT-Keycloak](https://github.com/telekom/kong-plugin-jwt-keycloak)
- [Gateway-Kong-Fork](https://github.com/telekom/gateway-kong-fork)

### Identity Provider

- [Identity-Iris-Hydra](https://github.com/telekom/identity-iris-hydra)

### Event-Driven Integration / Pubsub

- [Pubsub-Horizon-Cosmoparrot](https://github.com/telekom/pubsub-horizon-cosmoparrot)
- [Pubsub-Horizon-Go](https://github.com/telekom/pubsub-horizon-go)
- [Pubsub-Horizon-Spring-Parent](https://github.com/telekom/pubsub-horizon-spring-parent)
- [Pubsub-Horizon-Probe](https://github.com/telekom/pubsub-horizon-probe)

## Roadmap

The list below provides an outlook on the repositories that will be open-sourced in the future. It is subject to change.
The order of the items is not necessarily the order in which they will be open-sourced.

### Identity Provider

- Identity-Iris-Hydra-Charts

### Event-Driven Integration / Pubsub

- Pubsub-Horizon-Voyager

### Configuration Management

- Control Plane
- Rover
- API Catalog

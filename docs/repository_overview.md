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

| Name                                                                              |                                 Latest                                  |                                    Staging                                    |                               Production Ready                                |         Mandatory          |
|-----------------------------------------------------------------------------------|:-----------------------------------------------------------------------:|:-----------------------------------------------------------------------------:|:-----------------------------------------------------------------------------:|:--------------------------:|
| **[Gateway-Kong-Charts](https://github.com/telekom/gateway-kong-charts)**         |  **[latest](https://github.com/telekom/gateway-kong-charts/releases)**  |  **[9.1.2](https://github.com/telekom/gateway-kong-charts/releases/9.1.2)**   |  **[9.1.2](https://github.com/telekom/gateway-kong-charts/releases/9.1.2)**   |             ✅              |
| [Gateway-Kong-Image](https://github.com/telekom/gateway-kong-image)               |    [latest](https://github.com/telekom/gateway-kong-image/releases)     |     [1.3.0](https://github.com/telekom/gateway-kong-image/releases/1.3.0)     |     [1.3.0](https://github.com/telekom/gateway-kong-image/releases/1.3.0)     |             ✅              |
| [Gateway-Issuer-Service](https://github.com/telekom/gateway-issuer-service)       |  [latest](https://github.com/telekom/gateway-issuer-service/releases)   |                                       -                                       |                                       -                                       | for chart versions < 7.0.0 |
| [Gateway-Issuer-Service-Go](https://github.com/telekom/gateway-issuer-service-go) | [latest](https://github.com/telekom/gateway-issuer-service-go/releases) | [2.2.1](https://github.com/telekom/gateway-issuer-service-go/releases/v2.2.1) | [2.2.1](https://github.com/telekom/gateway-issuer-service-go/releases/v2.2.1) | for chart versions > 7.0.0 |
| [Gateway-Jumper](https://github.com/telekom/gateway-jumper)                       |      [latest](https://github.com/telekom/gateway-jumper/releases)       |       [4.3.1](https://github.com/telekom/gateway-jumper/releases/4.3.1)       |       [4.3.1](https://github.com/telekom/gateway-jumper/releases/4.3.1)       |             ✅              |
| [Gateway-Rotator](https://github.com/telekom/gateway-rotator)                     |      [latest](https://github.com/telekom/gateway-rotator/releases)      |      [1.0.0](https://github.com/telekom/gateway-rotator/releases/v1.0.0)      |      [1.0.0](https://github.com/telekom/gateway-rotator/releases/v1.0.0)      | for chart versions > 7.0.0 |

### Identity Provider

| Name                                                                                          |                                     Latest                                      |                                       Staging                                        |                                   Production Ready                                   | Mandatory |
|-----------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:|:---------:|
| **[Identity-Iris-Keycloak-Charts](https://github.com/telekom/identity-iris-keycloak-charts)** | **[latest](https://github.com/telekom/identity-iris-keycloak-charts/releases)** | **[1.2.10](https://github.com/telekom/identity-iris-keycloak-charts/releases/1.2.10)** | **[1.2.10](https://github.com/telekom/identity-iris-keycloak-charts/releases/1.2.10)** |     ✅     |
| [Identity-Iris-Keycloak-Image](https://github.com/telekom/identity-iris-keycloak-image)       |   [latest](https://github.com/telekom/identity-iris-keycloak-image/releases)    |   [1.1.2](https://github.com/telekom/identity-iris-keycloak-image/releases/1.1.2)    |   [1.1.2](https://github.com/telekom/identity-iris-keycloak-image/releases/1.1.2)    |     ✅     |

### Event-Driven Integration / Pubsub

| Name                                                                                    |                                    Latest                                    | Staging  | Production Ready | Mandatory |
|:----------------------------------------------------------------------------------------|:----------------------------------------------------------------------------:|:--------:|:----------------:|:---------:|
| **[Pubsub-Horizon-Helm-Charts](https://github.com/telekom/pubsub-horizon-helm-charts)** | **[latest](https://github.com/telekom/pubsub-horizon-helm-charts/releases)** | **tbd.** |     **tbd.**     |     ✅     |
| [Pubsub-Horizon-Comet](https://github.com/telekom/pubsub-horizon-comet)                 |      [latest](https://github.com/telekom/pubsub-horizon-comet/releases)      |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Galaxy](https://github.com/telekom/pubsub-horizon-galaxy)               |     [latest](https://github.com/telekom/pubsub-horizon-galaxy/releases)      |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Golaris](https://github.com/telekom/pubsub-horizon-golaris)             |     [latest](https://github.com/telekom/pubsub-horizon-golaris/releases)     |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Polaris](https://github.com/telekom/pubsub-horizon-polaris)             |     [latest](https://github.com/telekom/pubsub-horizon-polaris/releases)     |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Pulsar](https://github.com/telekom/pubsub-horizon-pulsar)               |     [latest](https://github.com/telekom/pubsub-horizon-pulsar/releases)      |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Quasar](https://github.com/telekom/pubsub-horizon-quasar)               |     [latest](https://github.com/telekom/pubsub-horizon-quasar/releases)      |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Starlight](https://github.com/telekom/pubsub-horizon-starlight)         |    [latest](https://github.com/telekom/pubsub-horizon-starlight/releases)    |   tbd.   |       tbd.       |   tbd.    |
| [Pubsub-Horizon-Vortex](https://github.com/telekom/pubsub-horizon-vortex)               |     [latest](https://github.com/telekom/pubsub-horizon-vortex/releases)      |   tbd.   |       tbd.       |   tbd.    |

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

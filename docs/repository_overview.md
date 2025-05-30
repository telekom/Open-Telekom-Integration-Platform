<!--
SPDX-FileCopyrightText: 2025 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Available Repositories

The list below provides an overview of the available repositories of the Open Telekom Integration Platform and its
components.

Every repository is linked to its GitHub page, where you can find more information about the specific component.

## Documentations

- [x] [Overarching documentation](https://github.com/telekom/Open-Telekom-Integration-Platform)
    - [x] [Event-Driven Integration / Pubsub](https://github.com/telekom/pubsub-horizon)

## Components and Recommended Versions

The matrix below lists the core components which are currently in use.
It further indicates the versions currently recommended for non-productive or production use.

### Gateway

| Name                                                                              |                              Latest Version                              | Non-Prod Version | Production Version | Mandatory |
|-----------------------------------------------------------------------------------|:------------------------------------------------------------------------:|:----------------:|:------------------:|:---------:|
| **[Gateway-Kong-Charts](https://github.com/telekom/gateway-kong-charts)**         |  **[latest](https://github.com/telekom/gateway-kong-charts/tree/main)**  |      **-**       |       **-**        |     ✅     |
| [Gateway-Kong-Image](https://github.com/telekom/gateway-kong-image)               |    [latest](https://github.com/telekom/gateway-kong-image/tree/main)     |        -         |         -          |     ✅     |
| [Gateway-Issuer-Service-Go](https://github.com/telekom/gateway-issuer-service-go) | [latest](https://github.com/telekom/gateway-issuer-service-go/tree/main) |        -         |         -          |     ✅     |
| [Gateway-Jumper](https://github.com/telekom/gateway-jumper)                       |      [latest](https://github.com/telekom/gateway-jumper/tree/main)       |        -         |         -          |     ✅     |
| [Gateway-Rotator](https://github.com/telekom/gateway-rotator)                     |      [latest](https://github.com/telekom/gateway-rotator/tree/main)      |        -         |         -          |     ❌     |

### Identity Provider

| Name                                                                                          |                                  Latest Version                                  | Non-Prod Version | Production Version | Mandatory |
|-----------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------:|:----------------:|:------------------:|:---------:|
| **[Identity-Iris-Keycloak-Charts](https://github.com/telekom/identity-iris-keycloak-charts)** | **[latest](https://github.com/telekom/identity-iris-keycloak-charts/tree/main)** |      **-**       |       **-**        |     ✅     |
| [Identity-Iris-Keycloak-Image](https://github.com/telekom/identity-iris-keycloak-image)       |   [latest](https://github.com/telekom/identity-iris-keycloak-image/tree/main)    |        -         |         -          |     ✅     |

### Event-Driven Integration / Pubsub

| Name                                                                                    |                                Latest Version                                 | Non-Prod Version | Production Version | Mandatory |
|:----------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------:|:----------------:|:------------------:|:---------:|
| **[Pubsub-Horizon-Helm-Charts](https://github.com/telekom/pubsub-horizon-helm-charts)** | **[latest](https://github.com/telekom/pubsub-horizon-helm-charts/tree/main)** |      **-**       |       **-**        |     ✅     |
| [Pubsub-Horizon-Comet](https://github.com/telekom/pubsub-horizon-comet)                 |      [latest](https://github.com/telekom/pubsub-horizon-comet/tree/main)      |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Galaxy](https://github.com/telekom/pubsub-horizon-galaxy)               |     [latest](https://github.com/telekom/pubsub-horizon-galaxy/tree/main)      |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Golaris](https://github.com/telekom/pubsub-horizon-golaris)             |     [latest](https://github.com/telekom/pubsub-horizon-golaris/tree/main)     |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Polaris](https://github.com/telekom/pubsub-horizon-polaris)             |     [latest](https://github.com/telekom/pubsub-horizon-polaris/tree/main)     |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Pulsar](https://github.com/telekom/pubsub-horizon-pulsar)               |     [latest](https://github.com/telekom/pubsub-horizon-pulsar/tree/main)      |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Quasar](https://github.com/telekom/pubsub-horizon-quasar)               |     [latest](https://github.com/telekom/pubsub-horizon-quasar/tree/main)      |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Starlight](https://github.com/telekom/pubsub-horizon-starlight)         |    [latest](https://github.com/telekom/pubsub-horizon-starlight/tree/main)    |        -         |         -          |     ✅     |
| [Pubsub-Horizon-Vortex](https://github.com/telekom/pubsub-horizon-vortex)               |     [latest](https://github.com/telekom/pubsub-horizon-vortex/tree/main)      |        -         |         -          |     ✅     |

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

<!--
SPDX-FileCopyrightText: 2025 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Build scripts for the Open-Telekom-Integration-Platform (O28M) components

This repository provides suggestions for build scripts for the [Open-Telekom-Integration-Platform](https://github.com/telekom/Open-Telekom-Integration-Platform) (O28M) components.  
**Attention:** The build scripts are not intended to be used as is. They are meant to be a starting point for your own build scripts.

The steps included are:

- Pulling the needed repositories
- Building the images
- Pushing them to a registry

The workflows are bundled per component and can be found in the `.github/workflows` directory.

## Components and Images

- [x] [Identity-Iris](../.github/workflows/identity-iris.yml):
  - [x] Identity-Iris-Keycloak (A modified Keycloak image)
- [x] [Gateway](../.github/workflows/gateway.yml):
  - [x] Gateway-Jumper (A sidecar container for the Gateway)
  - [x] Gateway-Issuer-Service-Go (A sidecar container for the Gateway)
  - [x] Gateway-Kong (A modified Kong image with several plugins and compatible with all Postgres versions >= 13, also 14+)
- [x] [Helper Images](../.github/workflows/helpers.yml):
  - [x] Bash-Curl (A helper image with pre-installed bash and curl to bootstrap and configure the Gateway)
  - [x] HAProxy (A HAProxy image to be used with Identity-Iris)
  - [x] Postgresql (A Postgres image to be used with Identity-Iris and Gateway-Kong for either database checks or as a local database deployment)

## Usage

To use the build scripts for your own purposes, it is recommended to fork this repository and adjust the workflows to
your needs.

### Docker-based builds (`_fetch_build_push_docker_image.yml`)

Used for components built with a Dockerfile (e.g. Gateway-Kong, Gateway-Issuer-Service-Go, Identity-Iris-Keycloak).

| Parameter                  | Description                                        | Default       | Required | Example                                                             |
|----------------------------|----------------------------------------------------|---------------|----------|---------------------------------------------------------------------|
| `source_repository`        | The repository to pull the source code from        | `''`          | Yes      | `telekom/gateway-kong-image`                                        |
| `source_branch`            | The branch to pull the source code from            | `main`        | No       | `main`                                                              |
| `source_dockerfile`        | The path to the Dockerfile                         | `Dockerfile`  | No       | `Dockerfile`                                            |
| `source_build_context`     | The context to build the image from                | `.`           | No       | `.`                                                                 |
| `source_build_args`        | List of build-time variables                       | `''`          | No       | `BASE_IMAGE_TAG=26.0.8`                                             |
| `target_image`             | The name of the image to build                     | `''`          | Yes      | `ghcr.io/${{ github.repository_owner }}/o28m/gateway-kong:latest`   |
| `target_architecture`      | The platforms/architectures to build the image for | `linux/amd64` | No       | `linux/amd64,linux/arm64`                                           |
| `target_registry`          | The registry to push the image to                  | `''`          | No       | `ghcr.io`                                                           |
| `target_registry_username` | The username to authenticate with the registry     | `''`          | No       | `${{ github.actor }}`                                               |
| `target_registry_password` | The password to authenticate with the registry     | `''`          | No       | `${{ secrets.GITHUB_TOKEN }}`                                       |

### Jib-based builds (`_fetch_build_push_jib_image.yml`)

Used for Java/Maven components that build container images with [Jib](https://github.com/GoogleContainerTools/jib) (e.g. Gateway-Jumper).

| Parameter                  | Description                                                        | Default       | Required | Example                                                             |
|----------------------------|--------------------------------------------------------------------|---------------|----------|---------------------------------------------------------------------|
| `source_repository`        | The repository to pull the source code from                        | `''`          | Yes      | `telekom/gateway-jumper`                                            |
| `source_branch`            | The branch to pull the source code from                            | `main`        | No       | `main`                                                              |
| `java_version`             | The Java version to use for the build                              | `25`          | No       | `25`                                                                |
| `java_distribution`        | The Java distribution to use for the build                         | `temurin`     | No       | `zulu`                                                              |
| `target_image`             | The name of the image to build                                     | `''`          | Yes      | `ghcr.io/${{ github.repository_owner }}/o28m/gateway-jumper:latest` |
| `target_platforms`         | Comma-separated target platforms; empty uses the POM configuration | `''`          | No       | `linux/amd64,linux/arm64`                                           |
| `target_registry`          | The registry to push to; empty builds in the local Docker daemon    | `''`          | No       | `ghcr.io`                                                           |
| `target_registry_username` | The username to authenticate with the registry                     | `''`          | No       | `${{ github.actor }}`                                               |
| `target_registry_password` | The password to authenticate with the registry                     | `''`          | No       | `${{ secrets.GITHUB_TOKEN }}`                                       |
| `maven_args`               | Additional Maven arguments to pass to the build                    | `''`          | No       | `-Djib.console=plain`                                               |

## Where are the images?

**We neither provide a public image registry nor pre-built images for the Open-Telekom-Integration-Platform.**

The images need to be built and pushed to a registry by yourself. The build scripts are meant to be a starting point for your own build scripts.

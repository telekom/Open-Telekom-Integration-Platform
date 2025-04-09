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
  - [x] Gateway-Issuer-Service (A sidecar container for the Gateway)
  - [x] Gateway-Kong (A modified Kong image with several plugins and compatible with all Postgres versions >= 13, also 14+)
- [x] [Helper Images](../.github/workflows/helpers.yml):
  - [x] Bash-Curl (A helper image with pre-installed bash and curl to bootstrap and configure the Gateway)
  - [x] HAProxy (A HAProxy image to be used with Identity-Iris)
  - [x] Postgresql (A Postgres image to be used with Identity-Iris and Gateway-Kong for either database checks or as a local database deployment)

## Usage

To use the build scripts for your own purposes, it is recommended to fork this repository and adjust the workflows to
your needs.

Parameters that can be adjusted are:

| Parameter                  | Description                                        | Default       | Required | Example                                                             |
|----------------------------|----------------------------------------------------|---------------|----------|---------------------------------------------------------------------|
| `source_repository`        | The repository to pull the source code from        | `''`          | Yes      | `telekom/gateway-jumper`                                            |
| `source_branch`            | The branch to pull the source code from            | `main`        | Yes      | `main`                                                              |
| `source_dockerfile`        | The path to the Dockerfile                         | `Dockerfile`  | No       | `Dockerfile`                                                        |
| `source_build_context`     | The context to build the image from                | `.`           | No       | `.`                                                                 |
| `source_build_args`        | List of build-time variables                       | `''`          | No       | `BASE_IMAGE_TAG=26.0.8`                                             |
| `target_image`             | The name of the image to build                     | `''`          | Yes      | `ghcr.io/${{ github.repository_owner }}/o28m/gateway-jumper:latest` |
| `target_architecture`      | The platforms/architectures to build the image for | `linux/amd64` | No       | `linux/amd64,linux/arm64`                                           |
| `target_registry`          | The registry to push the image to                  | `''`          | No       | `ghcr.io`                                                           |
| `target_registry_username` | The username to authenticate with the registry     | `''`          | No       | `${{ github.actor }}`                                               |
| `target_registry_password` | The password to authenticate with the registry     | `''`          | No       | `${{ secrets.GITHUB_TOKEN }}`                                       |

## Where are the images?

**We neither provide a public image registry nor pre-built images for the Open-Telekom-Integration-Platform.**

The images need to be built and pushed to a registry by yourself. The build scripts are meant to be a starting point for your own build scripts.
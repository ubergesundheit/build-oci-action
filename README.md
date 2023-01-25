# build-container-image-action

GitHub composite action for building container images the bCyber way

## Inputs

See `action,yml` for up to date documentation.

```yaml
inputs:
  registry_name:
    description: registry name (default ghcr.io)
    required: false
    default: "ghcr.io"
  registry_username:
    description: registry username
    required: true
  registry_password:
    description: registry password
    required: true
  image_repository:
    description: container image repository
    required: true
  dockerfile_path:
    description: path to Dockerfile (default ./Dockerfile)
    default: "./Dockerfile"
    required: false
  build_context:
    description: path to build context (default .)
    default: "."
    required: false
  push_to_registry:
    description: push to container registry (true/false)
    required: true
```

## Example Usage

```yaml
name: Build container image

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-container:
    name: Build container image
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Build and push container image
        uses: bCyberGmbH/build-container-image-action@main # or use a valid tag
        with:
          registry_username: ${{ github.repository_owner }}
          registry_password: ${{ secrets.GITHUB_TOKEN }}
          image_repository: ${{ github.repository }}
          dockerfile_path: deployment/Dockerfile.prod
          push_to_registry: ${{ github.event_name != 'pull_request' }}
```

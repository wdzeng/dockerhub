# Image Action

A GitHub action to build and push image to [Docker Hub](https://hub.docker.com) and [GitHub Container Registry (ghcr)](https://ghcr.io).

Three tags are pushed: `X.X.X`, `X.X` and `X`. Tag `latest` is optional.

## Prerequisites

- Dockerfile is placed at repository root location.

## Usage

Following example shows how to build project and build and push images to Docker Hub and GitHub Container Registry.

```yml
jobs:
  build:
    name: Build project and push image onto dockerhub
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Build project
        run: yarn install --frozen-lockfile && yarn build
      - uses: wdzeng/image@v1
        with:
          dockerhub-username: your-dockerhub-username
          dockerhub-password: ${{ secrets.DOCKERHUB_TOKEN }}
```

## Inputs

Unless otherwise noted with a default value, each input is required.

- `github-token`: token used to push image onto ghcr; required only if the repository has no write permission to ghcr.
- `dockerhub-username`: dockerhub username; default to github username
- `dockerhub-password`: dockerhub token
- `image`: image name; default to repository name
- `latest`: whether image with tag `latest` should be pushed; default to `false`
- `platforms`: platforms to build images; default to `linux/amd64,linux/arm64,linux/arm/v7`

You may need to set `github-token` for the first time the image is pushed to ghcr since it is tricky to give write permission to the repository.

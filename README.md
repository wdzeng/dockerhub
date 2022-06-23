# Image Action

A GitHub action to build project and push image onto dockerhub and ghcr.

Three tags are pushed: `X.X.X`, `X.X` and `X`. Tag `latest` is optional.

## Prerequisites

- Dockerfile is placed at repository root.

## Usage

Following example shows how to build project with pnpm and publish images to dockerhub.

```yml
jobs:
  build:
    name: Build project and push image onto dockerhub
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Install pnpm and dependencies
        uses: pnpm/action-setup@v2.2.2
        with:
          run_install: true
      - name: Build project
        run: pnpm build
      - uses: wdzeng/image@v1
        with:
          dockerhub-username: hyperbola
          dockerhub-password: ${{ secrets.DOCKERHUB_TOKEN }}
```

## Inputs

Unless otherwise noted with a default value, each input is required.

- `github-token`: token used to push image onto ghcr; required only if the repository has no write permission to ghcr.
- `dockerhub-username`: dockerhub username; default to github username
- `dockerhub-password`: dockerhub token
- `image`: image name; default to repository name
- `latest`: whether image with tag `latest` should be pushed; default to false

You may need to set `github-token` for the first time the image is pushed to ghcr since it is tricky to give write permission to the repository.

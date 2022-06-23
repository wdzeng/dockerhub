# Dockerhub Action

A GitHub action to build project and push image onto dockerhub.

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
      - uses: wdzeng/dockerhub@v1
        with:
          username: hyperbola
          password: ${{ secrets.DOCKERHUB_TOKEN }}
```

## Inputs

- `username`: dockerhub username; default to github username
- `image`: image name; default to repository name
- `latest`: whether image with tag `latest` should be pushed; default to false

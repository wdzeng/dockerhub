# Dockerhub Action

A GitHub action to build project and push image onto dockerhub.

Three tags are pushed: `X.X.X`, `X.X` and `X`. Tag `latest` is optional.

## Prerequisites

- Package manager must be pnpm, which required version is written in package.json.
- Command to build the repository is `pnpm build`.
- Dockerfile is placed at repository root.

## Usage

```yml
jobs:
  build:
    name: Build project and push image onto dockerhub
    runs-on: ubuntu-latest
    steps:
      - uses: wdzeng/dockerhub@v1
        with:
          username: hyperbola
          password: ${{ secrets.DOCKERHUB_TOKEN }}
```

## Inputs

- `username`: dockerhub username; default to github username
- `image`: image name; default to repository name
- `latest`: whether image with tag `latest` should be pushed; default to false

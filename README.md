# Image Action

A GitHub action to build and push image to [Docker Hub](https://hub.docker.com) and [GitHub Container Registry (ghcr)](https://ghcr.io).

For stable releases, four tags are pushed: `X.X.X`, `X.X`, `X`, and `latest`; for pre-releases, two tags are pushed: the project version and `edge`.

## How is Tag Generated?

For stable release, four tags will be added:

- `latest`
- `X`
- `X.Y`
- `X.Y.Z`

For pre-release, two tags will be added:

- `edge`
- entire version (without prefix), for example `1.0.0-alpha.1`

If the image has the variant name, replace `latest` to variant name, and add variant name as prefix to others. For example if variant name is `myvar`, then following tags are added:

- `myvar` (stable release only)
- `myvar-edge` (pre-release only)
- `myvar-1` (stable release only)
- `myvar-1.0` (stable release only)
- `myvar-1.0.0` (stable release only)
- `myvar-1.0.0-alpha.1` (pre-release only)

## Prerequisites

- Build context is the repository root (dockerfile is placed at the repository root).
- A package.json file is at the repository root.
- Project version is stated in the package.json.
  - Image tag will be determined by the version.
  - The version can have an optional `v` prefix.
  - The version must be a SemVer.
  - Example values:
    - `1.0.0`
    - `v1.0.0`
    - `1.0.0-alpha.1`
    - `v1.0.0-alpha.1`
- If the project has an author, it is stated in the package.json.
  - The author field will be copied to image label.
- If the project has a license, it is stated in the package.json.
  - The license name will be placed at image label.
- If the project has a description, it is stated in the package.json.
  - The description field will be copied to image label.

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
      - uses: wdzeng/image@v3
        with:
          dockerhub-username: your-dockerhub-username
          dockerhub-password: ${{ secrets.DOCKERHUB_TOKEN }}
```

## Inputs

Unless otherwise noted with a default value, each input is required.

- `image`: image name; default to repository name
- `github-token`: token used to push image onto ghcr; required only if the repository has no write permission to ghcr.
- `dockerhub-username`: dockerhub username; required only when pushing to dockerhub; default to github username
- `dockerhub-password`: dockerhub token; required only when pushing to dockerhub
- `variant`: image variant; default to none
- `dockerhub`: whether to push image to Docker Hub; default to `true`
- `platforms`: platforms to build images; default to `linux/amd64,linux/arm64,linux/arm/v7`
- `build-target`: build target; optional
- `build-args`: build arguments; one line per argument; optional

You may need to set `github-token` for the first time the image is pushed to ghcr since it is tricky to give write permission to the repository.

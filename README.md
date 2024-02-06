<!-- Badges -->

[![Issues](https://img.shields.io/github/issues/bcgov/action-deployer-api-gateway)](/../../issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/bcgov/action-deployer-api-gateway)](/../../pulls)
[![Apache 2.0 License](https://img.shields.io/github/license/bcgov/action-deployer-api-gateway.svg)](/LICENSE)
[![Lifecycle](https://img.shields.io/badge/Lifecycle-Experimental-339999)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

<!-- Reference-Style link -->

[issues]: https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue
[pull requests]: https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/working-with-your-remote-repository-on-github-or-github-enterprise/creating-an-issue-or-pull-request

# BC Gov API Gateway Deployer

GitHub Action. Deploy an API Gateway on the BC Government API Management solution supported by API Program Services. Most of the heavy lifting here is done in template configuration.

# Usage

```yaml
- uses: bcgov/action-deployer-api-gateway@main
  with:
    ### Required

    # Namespace of the API
    namespace: The namespace of you routes collection

    # API Client ID
    client_id: Namespace's Client ID from API Services Portal

    # API Client Secret
    client_secret: Namespace's Client Secret from API Services Portal

    # API Configuration File
    file: Template file (e.g. backend/service.gwa.yml)

    ### Typical / recommended

    # Use production environment
    # By default, set as true
    production: Set the environment to be used as the production environment
```

# Example

Publish a dev environment API for the end-user.

Create or modify a GitHub workflow, like below. E.g. `./github/workflows/pr-open.yml`

```yaml
name: Pull Request

on:
  pull_request:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  builds:
    permissions:
      packages: write
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
      - name: Publish API
        uses: bcgov/action-deployer-api-gateway@main
        with:
          namespace: api-namespace
          client_id: ${{ secrets.GWA_CLIENT_ID }}
          client_secret: ${{ secrets.GWA_CLIENT_SECRET }}
          file: backend/service.gwa.yml
```

# Example, Matrix

Read from multiple folders and publish each one.

Create or modify a GitHub workflow, like below. E.g. `./github/workflows/pr-open.yml`

```yaml
name: Pull Request

on:
  pull_request:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  builds:
    permissions:
      packages: write
    runs-on: ubuntu-22.04
    strategy:
      matrix:
        name: [api1, api2]
        include:
          - name: api1
            file: api1/api.gwa.yml
          - name: api2
            file: api2/service.yaml
    steps:
      - uses: actions/checkout@v3
      - name: Publish API
        uses: bcgov/action-deployer-api-gateway@main
        with:
          namespace: api-namespace
          client_id: ${{ secrets.GWA_CLIENT_ID }}
          client_secret: ${{ secrets.GWA_CLIENT_SECRET }}
          file: ${{ matrix.file }}
```

<!-- # Acknowledgements

This Action is provided courtesty of the Forestry Suite of Applications, part of the Government of British Columbia. -->

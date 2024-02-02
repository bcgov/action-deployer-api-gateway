<!-- Badges -->
[![Issues](https://img.shields.io/github/issues/bcgov-nr/action-gwa-publish)](/../../issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/bcgov-nr/action-gwa-publish)](/../../pulls)
[![MIT License](https://img.shields.io/github/license/bcgov-nr/action-gwa-publish.svg)](/LICENSE)
[![Lifecycle](https://img.shields.io/badge/Lifecycle-Experimental-339999)](https://github.com/bcgov/repomountie/blob/master/doc/lifecycle-badges.md)

# Action Deployer API Gateway

This action publishes an api through the BC Government Gateway gateway.

This is useful in CI/CD pipelines where you need to access a secret, get a vault token or anything vault related.

This tool is currently based on the existing documentation provided by 1team.

# Usage

```yaml
- uses: bcgov-nr/action-gwa-publish@main
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

Publish a test environment API for the end-user.

Create or modify a GitHub workflow, like below.  E.g. `./github/workflows/pr-open.yml`

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
        uses: bcgov-nr/action-gwa-publish@main
        with:
          namespace: api-namespace
          client_id: ${{ secrets.GWA_CLIENT_ID }}
          client_secret: ${{ secrets.GWA_CLIENT_SECRET }}    
          file: backend/service.gwa.yml

```

# Example, Matrix

Read from multiple folders and publish each one.

Create or modify a GitHub workflow, like below.  E.g. `./github/workflows/pr-open.yml`

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
        uses: bcgov-nr/action-gwa-publish@main
        with:
          namespace: api-namespace
          client_id: ${{ secrets.GWA_CLIENT_ID }}
          client_secret: ${{ secrets.GWA_CLIENT_SECRET }}    
          file: ${{ matrix.file }}

```


<!-- # Acknowledgements

This Action is provided courtesty of the Forestry Suite of Applications, part of the Government of British Columbia. -->

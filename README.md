# shared-actions
Shared CI/CD across all labs products.

## Overview
This repository contains shared CI/CD actions for all labs products, including the following:
- React: build and publish
- Django: build and publish
- Docker Publish
- Deployment: generate manifests and deploy to Kubernetes
- Labs Application: consolidated build, publish, and deploy for standard React-Django labs applications

## Usage
To use shared actions, add the following to your workflow file:

```yaml
- uses: pennlabs/shared-actions/.github/workflows/[workflow-name].yaml@main
  with:
    [parameter]: [value]
```

> NOTE: Information such as githubRefs, environment variables, and secrets must be passed into the shared action as
inputs.

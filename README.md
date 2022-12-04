# shared-actions
Shared CI/CD across all labs products.

## Overview
This repository contains shared CI/CD actions for all labs products, including the following:
- React: build and publish
- Django: build and publish
- Docker Publish
- Deployment: generate manifests and deploy to Kubernetes

## Usage
To use shared actions, add the following to your workflow file:

```yaml
- uses: pennlabs/shared-actions/.github/workflows/[workflow-name].yaml@[version tag]
  with:
    [parameter]: [value]
```

Specifically, use each job as follows:

### django-check
```yaml
  name: "<JOB_NAME>"
    uses: pennlabs/shared-actions/.github/workflows/django-check.yaml@v0.1
    with:
      projectName: <PROJECT_NAME>
      path: <PATH_TO_DJANGO_FOLDER>, root directory (.) by default>
      flake: <true/false>, whether to use flake linting>
      black: <true/false>, whether to run black on all python files>

```

### react-check
```yaml
  name: "<JOB_NAME>"
    uses: pennlabs/shared-actions/.github/workflows/react-check.yaml@v0.1
    with:
      path: <PATH_TO_REACT_FOLDER>, root directory (.) by default>

```

### docker-publish
```yaml
  name: "<JOB_NAME>"
  uses: pennlabs/shared-actions/.github/workflows/docker-publish.yaml@v0.1
    with:
      # Inputs
      imageName: "<PRODUCT_NAME_BACKEND>"
      githubRef: ${{ github.ref }}
      gitSha: ${{ github.sha }}

      # Optional inputs
      
      # Path to the docker context
      path: backend
      
      # Path to the dockerfile (relative to `path` variable)
      dockerfile: <DOCKERFILE_PATH>
      
      # If enabled, will cache_from the latest version of the docker image.
      cache: <true/false>, 

      secrets: 
        DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
        DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
    
    needs: backend-check
```

### deployment
```yaml
  deploy:
    name: "Deploy"
    uses: pennlabs/shared-actions/.github/workflows/deployment.yaml@v0.1.1j

    with:
      githubRef: ${{ github.ref }}
      gitSha: ${{ github.sha }}

    secrets:
      AWS_ACCOUNT_ID: ${{ secrets.AWS_ACCOUNT_ID }}
      GH_AWS_ACCESS_KEY_ID: ${{ secrets.GH_AWS_ACCESS_KEY_ID }}
      GH_AWS_SECRET_ACCESS_KEY: ${{ secrets.GH_AWS_SECRET_ACCESS_KEY }}

    needs:
      - <BACKEND_PUBLISH_JOB_NAME>
      - <FRONTEND_PUBLISH_JOB_NAME>
```

> NOTE: Information such as githubRefs, environment variables, and secrets must be passed into the shared action as
inputs.

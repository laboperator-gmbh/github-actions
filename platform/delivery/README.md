# Build and Deploy

It automatically builds and deploy the latest commit.

Trigger the CircleCI build in the latest commit of the target branch.
After CircleCI completes, it will create a branch in the platform-deployment repository
and commit to it with the following commit message: "[input.command]{input.apps} Created by Github Actions from ${{ github.event.repository.name }}"
The commit will use the latest short-commit in the chartVersion.

# Inputs and Outputs

Check the inputs and outputs in the [action file](action.yml).

# Usage Example

```
name: Build & Deploy

on:
  workflow_dispatch:
    inputs:
      command:
        description: 'Command send to the platform-repository'
        required: true
        default: 'deploy'
        type: choice
        options:
          - deploy
      apps:
        description: "Application's Acronyms to deploy. Check the full list of available applications in
        https://github.com/labforward/platform-deployment/blob/main/config/installation/activation.properties"
        type: string
        required: true
        default: '{fos}'

jobs:
  create-platform-branch:
    name: 'Build And Trigger Platform Deployment'
    runs-on: ubuntu-22.04
    steps:
      - name: 'Build with CircleCI and Deploy via Platform'
        uses: labforward/platform-gha/platform/delivery@1
        with:
          cci_token: ${{ secrets.CCI_TOKEN }}
          docker_build_job_name: 'docker-build-commit-workflow'
          gh_pat: ${{ secrets.GH_PAT }}
          apps: ${{ inputs.apps }}
          chart_name: "file-object-storage-chart.yaml"
```

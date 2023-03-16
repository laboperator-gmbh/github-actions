# Create Branch in platform-deployment Repository

Creates a new branch named gha-{current-branch}.
Then, creates a new commit replacing the chartVersion with the latest commit hash.
If the branch already exists, then it will create only the new commit.

# Inputs and Outputs

Check the inputs and outputs in the [action file](action.yml).

# Usage Example

The example is creating a new branch in the platform-deployment repository via a manual trigger in the Actions tab.
Also, it updates the _chartVersion_ in the _chart-name_ manifest file with the short-commit hash of the latest commit of the target branch.

```
name: Create Platform Branch

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
    name: 'Commit chartVersion to Platform Branch'
    runs-on: ubuntu-22.04
    steps:
      - name: 'Create platform-deployment Branch'
        uses: labforward/platform-gha/platform/branch/create@1
        with:
          gh_pat: ${{ secrets.GH_PAT }}
          apps: ${{ inputs.apps }}
          chart_name: "file-object-storage-chart.yaml"
```

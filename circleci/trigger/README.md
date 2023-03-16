# CircleCI Build Trigger

It trigger the CircleCI build pipeline in the target branch.
Intended to build all docker images and charts required to deploy the application.

# Inputs and Outputs

Check the inputs and outputs in the [action file](action.yml).

# Usage Example

The example is creating a manual trigger to the circleci and adding the build parameter by default.
The manual trigger will appear in the Actions tab of GitHub's UI.

```
name: Trigger CircleCI Build

on:
  workflow_dispatch:

jobs:
  circleci-docker-build:
    name: 'Trigger CircleCI Build'
    runs-on: ubuntu-22.04
    steps:
      - name: 'Trigger CircleCI Build'
        uses: labforward/platform-gha/circleci/trigger@1
        with:
          cci_token: ${{ secrets.CCI_TOKEN }}
          docker_build_job_name: 'docker-build-commit-workflow'
```

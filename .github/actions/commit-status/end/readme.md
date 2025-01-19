# Commit Status End

How to use it

```yml
name: E2E
on:
  workflow_dispatch:
    inputs:
      git_ref:
        type: string
      region:
        type: choice
        options:
          - "us-east-1"
          - "us-east-2"
          - "us-west-2"
          - "eu-west-1"
        default: "us-east-2"
  workflow_call:
  
jobs:
  run-suite:
    permissions:
      id-token: write # aws-actions/configure-aws-credentials@v4.0.1
      statuses: write # ./.github/actions/commit-status/start
    name: suite-${{ inputs.suite }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@9bb56186c3b09b4f86b1c65136769dd318469633 # v4.1.2
        with:
          ref: ${{ inputs.git_ref }}
      - if: always() && github.event_name == 'workflow_run'
        uses: ./.github/actions/commit-status/start
        with:
          name: ${{ github.workflow }} (${{ inputs.k8s_version }}) / e2e (${{ inputs.suite }})
          git_ref: ${{ inputs.git_ref }}
```
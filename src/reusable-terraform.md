# Re-usable Terraform Workflow

- [Terraform Workflow](.github/workflows/terraform.yml)

- Does not use **lit-terraform**.

## Requirements

- Create an environment in the `caller` github repository
    1. Go to the repository `Settings` page
    2. Click `Environments` and then add new environments

## Usage example

Create one file per environment - [example](https://github.com/CondeNast/dogfood-gp-app-v3/pull/173/files#diff-23f80768a609e8b706d4abbd9c8e713b16988d231af23c312541f7762e2ce393).

```yaml
name: change-me

on:
  push:
    paths:
    # which paths should trigger this workflow
      - "terraform/*"
    branches:
    # which branches should trigger this workflow
      - "*"

jobs:
  terraform:
    uses: CondeNast/global-workflows/.github/workflows/terraform.yml
    with:
      # Desired Terraform version
      version: "1.2.6"

      # folder with terraform variables
      infra_dir: dogfood-nonprod/ap-northeast-1
      working_dir: infra

      # aws region
      aws_default_region: us-east-1
      # AWS account ID on which the resources are created
      account_id: 380688878008

      # Github environment name
      environment_plan: dogfood-nonprod-readonly
      environment_apply: dogfood-nonprod-protected

      # Branch that is used to run terraform apply
      # Default value is main
      main_branch: main
      
      enable_branch_apply: true  # Default value is false
```

## Enhancements of the workflow

- Introduce TFSEC for security reasons -- it scans Terraform code for security vulnerabilities and misconfigurations.

- Explore [INFRACOST](https://www.infracost.io/) to get a better idea about the cost being genereated by the developers.

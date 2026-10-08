# Terraform plan & apply pipelines (Will be deprecated in future)

[Reusable workflow definition](.github/workflows/lit-terraform.yml)

## Requirements

- Be familiarized with `lit-terraform` and how to setup a terraform project.
    - [This lab should help.](https://github.com/CondeNast/labs/blob/master/labs/setup-terraform-project.md)
- Create a secret on the `caller` github repository for pulling docker images from quay.io
    1. Go to the repository `Settings` page
    2. Click `Secrets` and then add a new repository secret
    3. Add the following secrets:
        - `QUAY_USER` 
        - `QUAY_TOKEN`
        > These secrets might change in the near future when we move from Quay.io to ECR
- Create an environment in the `caller` github repository
    1. Go to the repository `Settings` page
    2. Click `Environments` and then add new environments
    3. Each environment should have the following secrets set: 
        - `AWS_ACCESS_KEY_ID`
        - `AWS_SECRET_ACCESS_KEY`

## Usage example

Create one file per environment - [you can check this repository as a design pattern example](https://github.com/CondeNast/global-dns/tree/master/.github/workflows).

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
    uses: CondeNast/global-workflows/.github/workflows/lit-terraform.yml@v1
    with:
      # lit-terraform version
      version: 1-0.12.31

      # folder with terraform variables
      infra_dir: global-platform-nonprod-na

      # aws region
      aws_default_region: us-east-1

      # Github environment name
      environment: global-platform-nonprod-na

      # Branch that is used to run terraform apply
      # Default value is main
      main_branch: master

      # Use this setting if you need to apply your 
      # terraform on every branch instead of applying
      # only on the main_branch.
      #
      # These settings are mutually exclusive, meaning
      # if you set `main_branch` you shouldn't set
      # `enable_branch_apply`.
      # 
      # Default value is false
      enable_branch_apply: true
    secrets:
      docker_user: ${{ secrets.QUAY_USER }}
      docker_token: ${{ secrets.QUAY_TOKEN }}
      aws_access_key_id: ${{ secrets.AWS_ACCESS_KEY_ID }}
      aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

---
title: "GitHub Repos, Terraform and Actions!"
date: 2022-10-01T17:22
tagline: "Automated creation/management of GitHub Repos using Terraform"
header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/lacblanc.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Bart Parka**](https://www.instagram.com/bart_parka/)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/terraform-github-repos"
categories:
  - blog
tags:
  - GitHub Actions
  - Terraform
---

Automating and standardising GitHub repository deployments using Terraform and GitHub Actions.

## Workflow

So the `actions.yml` I put together for this is really simple, basically everytime I push to master, actions will be triggered where Terraform runs and deploys the new infrastructure.
This is super useful if you're looking to deploy standard config/settings across your repositories (think branch protections, hooks, templates etc.). Also, it gives your team self-service ability to create repositories through IaC - rather than manually each time.

We are using the [GitHub Terraform Provider](https://registry.terraform.io/providers/integrations/github/latest/docs) - I found it works best with `github.com` but have managed to use it with a GitHub Enterprise (GHE) instance, just be mindful of what is possible with your GHE version.

Interesting things to note:
* Unfortunately the `GITHUB_TOKEN` that comes pre-configured does not have sufficient permissions to interact with the GitHub API so doesn't work here
* The remaining secrets are attached to the repository under Settings -> Secrets -> Actions
* Further improvement would be to pause for plan review and/or drop the plan in GitHub comments

```yml
name: Deploy Repo

on:
  push:
    branches:
      - master

env:
  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  TF_VAR_gh_token: ${{ secrets.GH_TOKEN }}
  AWS_ACCESS_KEY_ID:  ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY:  ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        
jobs:
  tf_fmt:
    name: Deploy Terraform
    runs-on: ubuntu-latest
    steps:
    - uses: hashicorp/setup-terraform@v2

    - name: Checkout Repo
      uses: actions/checkout@v1

    - name: Terraform Init
      id: init
      run: terraform init -input=false -backend-config=vars/personal/backend.hcl

    - name: Terraform Validate
      id: validate
      run: terraform validate -no-color

    - name: Terraform Plan
      id: plan
      run: terraform plan -input=false -no-color -var-file=vars/common.tfvars -var-file=vars/personal/vars.tfvars

    - name: Terraform Apply
      id: apply
      run: terraform apply -input=false -no-color -auto-approve -var-file=vars/common.tfvars -var-file=vars/personal/vars.tfvars
```
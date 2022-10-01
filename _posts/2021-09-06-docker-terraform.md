---
title: "Part 2: Containerising your Development Tools"
date: 2021-09-06T16:55
tagline: "Picking up where we left of last time. Now adding Terraform to our image while still leveraging aws-vault for credentials."
header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/docker-terraform.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Bart Parka**](https://www.instagram.com/bart_parka/)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/pages-containerised-terraform"
categories:
  - blog
tags:
  - Docker
  - AWS
  - aws-vault
  - Security
  - Terraform
  - Infrastructure As Code
---

In this post we will continue setting up our development image. We will install Terraform onto the image and authenticate against AWS using aws-vault. We will then deploy some resources to two accounts, making use of AWS Profiles.

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktopDocker Desktop): `20.10.7`
- 2x AWS Account
- Completion of [Part 1: Securely Storing your AWS Credentials](https://bart-parka.github.io/blog/docker-aws-vault/)

## Guide

Assuming you named and tagged the image from the previous post as, `awscli:test`, we can add Terraform to the image easily in a `Dockerfile`:

```Dockerfile
FROM awscli:test

ARG TERRAFORM_VERSION

# Install Terraform
# Verify the signature file is untampered
# Verify the SHASUM matches the archive
COPY assets/hashicorp-public.key /
RUN gpg --import hashicorp-public.key  && \
    curl -sLO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    curl -sLO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_SHA256SUMS && \
    curl -sLO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_SHA256SUMS.sig && \
    gpg --verify terraform_${TERRAFORM_VERSION}_SHA256SUMS.sig terraform_${TERRAFORM_VERSION}_SHA256SUMS && \
    echo "$(cat terraform_${TERRAFORM_VERSION}_SHA256SUMS | grep terraform_${TERRAFORM_VERSION}_linux_amd64.zip)" | sha256sum -c && \
    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    rm terraform_${TERRAFORM_VERSION}* \
       hashicorp-public.key && \
    mv terraform /usr/local/bin
```

With that done, we can test the new image using the docker compose pattern. See the [GitHub](https://github.com/bart-parka/pages-containerised-terraform) repo for this page for an example. You can choose the Terraform version to be installed by modifying `.env`. Run `docker compose up` to build and test the new image.

Once again you may want to add an alias to make it easier to run terraform commands (make sure you update your `.zshrc` or `.bash_profile` to create the aliases when you open a new shell):

```
alias terraform-container="docker run --rm -it -v <PATH TO REPO ROOT>/assets/aws/:/root/.aws -v test-vault-volume:/root/.awsvault/keys -v $(pwd):/tfmount -e AWS_VAULT_FILE_PASSPHRASE=\$(security find-generic-password -a \${USER} -s test-vault-volume -w) -e AWS_VAULT_BACKEND=file terraform:test"
```

Note above we've added a mount to the current directory (`-v $(pwd):/tfmount` and `-w=/tfmount`) , so that Terraform can pick up `.tf` files when executing in your project directory.

Check your alias works by running `terraform-container --version`.

## Deploying a new account and switching roles

Clone this example [Terraform repository](https://github.com/bart-parka/terraform-module-org-accounts). 

Note the below Terraoform module is meant to be run with a fresh account that does not belong to or own an organisation. The Terraform will create an Organisation, with the current account as the root. It will then create a new account and assign it as a memeber of the newly created organisation. We also assume that the [access key you have saved in vault](https://bart-parka.github.io/blog/docker-aws-vault/) is for an IAM user with admin permissions in the root account (or at least one with sufficient IAM/Org permissions) and has MFA switched on. This adds a little bit of complexity as Terraform running inside the container won't prompt us for the MFA token that is required, we need to save it before hand. We can once again use the `aws-vault` alias for this, assuming you previously added it to your bash profile (e.g. `~/.bash_profile` or `~/.zshrc`).

Run `aws-vault-container exec id -- echo "Token Saved"` to confirm we have an active session token.

Run `terraform-container init` from the `/examples/two-accounts` then `terraform-container apply`.

This will deploy the following:

1. Organisation (Account 1)

2. AWS account belonging to above Organisation (Account 2)

3. IAM Group for Organisation admins (Account 1)

4. IAM Policy for above group 

5. Addition of Account 1 IAM user to Org admin group

Once we know the account id of our new account, we can add a profile to our aws config and credential files, remembering to include `mfa_serial`. See assets in [`pages-containerised-terraform`](https://github.com/bart-parka/pages-containerised-terraform) repo for example.

You can test if your setup has worked by trying to role switch into second account. Make sure you update your `aws-container` alias to pick up the newly edited credential file.

```
alias aws-container='docker run --rm -it -v <PATH TO REPO ROOT>/assets/aws/:/root/.aws -v test-vault-volume:/root/.awsvault/keys -e AWS_VAULT_FILE_PASSPHRASE=$(security find-generic-password -a ${USER} -s test-vault-volume -w) -e AWS_VAULT_BACKEND=file awscli:test'
```

noting that the `<PATH to REPO ROOT>` is now pointing at the `pages-containerised-terraform` repo rather than the one in the previous post.

```
alias aws-container='docker run --rm -it -v /Users/bart/github/bartparka/pages/pages-containerised-terraform/assets/aws/:/root/.aws -v test-vault-volume:/root/.awsvault/keys -e AWS_VAULT_FILE_PASSPHRASE=$(security find-generic-password -a ${USER} -s test-vault-volume -w) -e AWS_VAULT_BACKEND=file awscli:test
```

Test you can role switch successfully:

`aws-container sts get-caller-identity --profile account2`

## Cleaning up

Deleting the additional AWS account we have created is a surprisingly arduous process. Currently this is not practical to do in automation (and Control Tower which is suppossed to simplify multi-account environments has no API access)

1. Find the accounts root user e-mail address
2. Sign out or open an incognito tab and request a password reset
3. Open the e-mail sent and click the provided link
4. Set a new password and log in with the new password
5. Set the account contact information (if not inherited from Organizations)
6. Accept the AWS Customer Agreement (if not inherited from Organizations)
7. Provide a valid payment method
8. Make it the default payment method
9. Verify yourself by phone - you can do this going to the "Complete the account sign-up steps" link when trying to leave the org from the child account
10. Remove the account from Organizations within the master account
11. Delete the account from the root user of the account

Reference: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_remove.html#orgs_manage_accounts_remove-from-master








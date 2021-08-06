---
title: "Part 1: Securely Storing your AWS Credentials"
date: 2021-07-30T14:09
tagline: "A plug-and-play pattern for secure storage of AWS access keys and convenient packaging of developer tools."
header:
  overlay_image: /assets/docker-aws-vault.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Stefan Miron**](https://www.instagram.com/stefanmironphotography)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/pages-securely-storing-aws-credentials"
categories:
  - blog
tags:
  - Docker
  - AWS
  - aws-vault
  - Security
---

The default practice for an IAM user to gain programmatic access to AWS is through generating AWS access keys. These consist of an ID and secret key which are stored on the user's machine (you can read more [here](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)).

While you should use IAM roles instead of long-term access keys wherever possible (as described in the link above), there will no doubt be scenarios where access keys will still need to be generated. The dangers of this are well summarised in this [blog post](https://99designs.com.au/blog/engineering/aws-vault/). Storing credentials on an engineer's workstation exposes a number of attack vectors. These are quoted verbatim from 99designs' blog post:

- Stolen or misplaced engineer’s laptops with credentials on them

- Malware on engineer laptops that steal credentials

- Accidentally published AWS credentials on Github or elsewhere

- Ex-employees who haven’t had credentials revoked

The mitigations for most of these are well known, such as following the principle of least privilege, limiting blast-radius by segregating teams into dedicated AWS accounts with limited cross-account access as well as securing the credentials on engineer's machines.

In this post I will outline how to use the [aws-vault](https://github.com/99designs/aws-vault) to securely store AWS credentials on developer machines as well as how combining this approach with Docker has the potential to drastically reduce onboarding time for new engineers.

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktopDocker Desktop): `20.10.7`
- AWS Account

## Guide

If you look at the `Dockerfile` in [GitHub repository](https://github.com/bart-parka/pages-securely-storing-aws-credentials) for this post, you will note the following:

- Note the `AL2_VERSION` has to be a global build argument in order to be used in the `FROM` instruction.

- We are passing build arguments for `AWS_CLI_VERSION` and `AWS_VAULT_VERSION`.

- Build arguments are passed in automatically during `docker compose` using `.env` file.

- We are using Amazon Linux 2 as the base image. We could use Alpine to reduce the image size but using AL2 will make it easier to install some other tooling we will discuss in future posts.

- We are checking the signature of the downloaded zips to check the package hasn't been tampered with. This is good practice, you can read more [here](https://www.gnupg.org/gph/en/manual/x135.html).

- We are installing both awscli and aws-vault onto the image.

- If you run `docker compose`, you can see the image is built and tests ran. We use the same approach to testing our container as discussed [here](https://bart-parka.github.io/blog/docker-inspec/). The test for the `aws-vault` is a strange one due to what is seemingly a [bug](https://github.com/inspec/inspec/pull/4548) with the command output.

### Setup of AWS credentials

It's now time to setup the image so that you can use it to store your AWS credentials and access your AWS resources programmatically. We have a choice of running a number of backends with `aws-vault` but to keep it simple we will use an encrypted file. We can set this using the `AWS_VAULT_BACKEND=file` environment variable. This backend file needs to be persisted between runs (as it contains your credentials) so we could either put it on the host file system and mount the directory or put it in a docker volume. In this post I have chosen the latter approach, as:

- Volumes have better higher performance than bind mounts from Mac and Windows hosts.

- Volumes can be more safely shared among multiple containers.

- Data in a volume, in this case sensitive credentials, is less readily accessible from the host machine (as it exists in the Docker VM).

- Volumes are easier to back up or migrate than bind mounts.

you can read more about bind mounts [here](https://docs.docker.com/storage/bind-mounts/) and docker volumes [here](https://docs.docker.com/storage/volumes/).

To create a docker volume, run:

`docker volume create test-vault-volume`,

where `test-vault-volume` is the name of the volume.

Now, we will need to secure the encrypted file with a passphrase. We can tell `aws-vault` what the passphrase is by using the `AWS_VAULT_FILE_PASSPHRASE` environment variable. If you are on macOS, I suggest using Keychain here. We can create a dedicated password for this using:

`security add-generic-password -a ${USER} -s test-vault-volume -w <YOUR-PASSWORD>`,

we can then retrieve the password using:

`security find-generic-password -a ${USER} -s test-vault-volume -w`.

Quick checkpoint, so far we should have:

- A successful run of `docker compose`, proving we have aws-vault and awscli installed on the image with correct versions.

- A docker volume created, ready to store the encrypted vault file.

- A password in macOS keychain, which we can access using `security find-generic-password` and which we will use as the passphrase for the encrypted vault file.

Before we continue, at this stage it is a good idea to create some aliases. As we are aiming to run `aws-vault` and `aws` commands from a container, we will actually run `docker run` commands on the host system. These can get complicated quickly, so it's worth abstracting some of the complexity away through aliasing. Add the following to your bash profile (e.g. `~/.bash_profile` or `~/.zshrc`):

`alias aws-vault-container="docker run --rm -it -v <PATH TO REPO ROOT>/assets/aws/:/root/.aws -v test-vault-volume:/root/.awsvault/keys -e AWS_VAULT_FILE_PASSPHRASE=\$(security find-generic-password -a \${USER} -s test-vault-volume -w) -e AWS_VAULT_BACKEND=file --entrypoint=aws-vault awscli:test"`.

Run `source ~/.zshrc` and run `aws-vault-container --version` to confirm the alias loaded properly. You should get the `aws-vault` version returned.

Similarly, add the following to your profile:

`alias aws-container="docker run --rm -it -v <PATH TO REPO ROOT>/assets/aws/:/root/.aws -v test-vault-volume:/root/.awsvault/keys -e AWS_VAULT_FILE_PASSPHRASE=\$(security find-generic-password -a \${USER} -s test-vault-volume -w) -e AWS_VAULT_BACKEND=file awscli:test"`,

run `source ~/.zshrc` again and confirm `aws-container --version` returns the awscli version.

Ok, with that behind us we are ready to finally store our AWS access keys. Go ahead and generate an access key in AWS Management Console, then run:

`aws-vault-container add id`,

then insert your access key ID and secret key when prompted.  

Check you can access your account using the container:

`aws-container sts get-caller-identity --profile account1`

Ok. Done for now. This puts us in a great position to package up some more tools in the container (like Terraform or Packer). With the way we have set everything up here, we can still access credentials through `aws-vault` even when using tools like Terraform. We can also extend this configuration to easily access other accounts via `AssumeRole`. We will cover this in a future post.

Thanks!



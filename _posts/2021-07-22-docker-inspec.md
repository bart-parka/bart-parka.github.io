---
title: "Testing Docker in Development"
date: 2021-07-22T15:52
tagline: "Test your Docker containers locally using Docker Compose and Inspec."
header:
  overlay_image: /assets/docker-inspec.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Stefan Miron**](https://www.instagram.com/stefanmironphotography)"
categories:
  - blog
tags:
  - Docker
  - Inspec
  - Testing
  - Compose
---

Want an easy way to run Inspec tests against a Docker container in development, without worrying about local Ruby/Chef Inspec installations? Read on.

## Requirements

- Docker Desktop: `20.10.5` (https://www.docker.com/products/docker-desktop)

## Guide

There are a number of ways you could go about this. Each has its pros and cons.

1. Install Inspec on your local workstation inspect a Docker container via the Docker API. (See [Compliance for Containers](https://blog.chef.io/docker-container-compliance-with-inspec))

    - **Pros**: Simple and quick if you have an existing Ruby/Inspec environment.

    - **Cons**: Ruby/Inspec versions on your local machine may differ to what was intended. May not want to install Ruby/Inspec.

2. Install an `ssh-agent` on your container and use the Inspec image to scan your container via SSH (See [chef/inspec image](https://hub.docker.com/r/chef/inspec)).

    - **Pros**: Most secure.

    - **Cons**: Complicated. Need to install an `ssh-agent`, setup network, open ports and handle ssh keys.

3. Give the Inspec container access to the host machines `docker.sock`, put on the same network. You can then target the container to be tested in the same manner as option 1. (See [The Socket Solution](http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/)).

    - **Pros**: Relatively simple, can pin Inspec version and build and test together using container orchestration (See Example repo for Docker Compose).

    - **Cons**: Potentially insecure, the Inspec container has access to the host machine's Docker socket (so could compromise other containers, therefore this option shouldn't be used in production)

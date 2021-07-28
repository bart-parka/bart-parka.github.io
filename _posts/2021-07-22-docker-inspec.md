---
title: "Testing Docker in Development"
date: 2021-07-22T15:52
tagline: "Test your Docker containers locally using Docker Compose and Inspec."
header:
  overlay_image: /assets/docker-inspec.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Stefan Miron**](https://www.instagram.com/stefanmironphotography)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/pages-testing-docker-in-development"
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

- Docker Desktop: `20.10.7` (https://www.docker.com/products/docker-desktop)

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

This example covers option 3.

### Example Code

```
├── Dockerfile
├── LICENSE
├── README.md
├── assets
│   └── index.html
├── docker-compose.yml
└── test
    └── default
        ├── README.md
        ├── controls
        │   └── example.rb
        └── inspec.yml

```

**Tip**: You can generate a skeleton Inspec profile using `inspec init profile <profile-name>`
{: .notice--info}

As you can see in the GitHub repository, we can use Docker compose to orchestrate the creation of the two containers. If we assign a container name and bind mount the `docker.sock` to the Inspec container, we can simply use the Docker API to point to the web server container that we are testing:

```yaml
version: "3.9"
services:
  webserver:
    build:
      dockerfile: Dockerfile
    image: webserver:test
    ports:
      - "80"
    container_name: compose-web
  inspec:
    image: "chef/inspec"
    container_name: compose-inspec
    volumes:
      - type: bind
        source: ./
        target: /workdir
      - type: bind
        source: /var/run/docker.sock
        target: /var/run/docker.sock
    working_dir: /workdir
    depends_on:
      - "webserver"
    entrypoint: inspec exec test/default -t docker://compose-web --chef-license accept-silent

```

**Warning**: If you experience permission errors, you may need to change permissions on `/var/run/docker.sock`.
{: .notice--warning}

A few things to note:

* I am only assigning a `CONTAINER_PORT` (syntax `CONTAINER_PORT:HOST_PORT`) to the webserver. This means I won't be able to access the web server from my host machine, but the Inspec container (as it is on the same network) will be able to access web server's IP by the container name. This is apparent in the test case (note the URL):

  ```ruby
  describe http('http://compose-web:80') do
    its('status') { should cmp 200 }
    its('body') { should match 'I am an Nginx container!' }
  end
  ```

* I have to mount the local directory against the Inspec container so that it can parse the profile under `test/default`

**Warning**: Make sure mounting the current directory is allowed in Docker Desktop. You can configure shared paths from `Docker -> Preferences... -> Resources -> File Sharing`.
{: .notice--warning}

* The tests should only run after the web server image is built and running, hence the `depends_on`

* As previously mentioned, by mounting the docker socket we can refer to the container name `compose-web` in our `inspec` command

Running `docker compose up` from the root of the repo will yield:

<figure>
  <img src="/assets/images/2021-07-22-docker-inspec/docker-compose-up.png">
  <figcaption>Successful run.</figcaption>
</figure>

Notice the `GET` request reaching Ngin on the `compose-web` container. This is Inspec running the test case.

Exit with `Ctrl + C` then run `docker compose down` to clean-up.

That's it!

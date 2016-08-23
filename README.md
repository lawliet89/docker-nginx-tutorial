# Docker Nginx Reverse Proxy with Let's Encrypt Certificate Tutorial

This repository contains an example `docker-compose.yml` file to set up an nginx reverse proxy
with an auto-renewing [Let's Encrypt](https://letsencrypt.org/) TLS certificate for your web application.

## Pre-requisites
 - [Docker](https://docs.docker.com/engine/installation/)
 - [Docker Compose](https://docs.docker.com/compose/install/) - optional -- to run the example
 - A working sub-domain
 - A working knowledge of Docker, and especially about
 [volumes](https://docs.docker.com/engine/tutorials/dockervolumes/)

## Submodule
When you clone this repository, don't forget to clone the submodule too!

```bash
git submodule init
git submodule update
```

## Configuration
To run the example `docker-compose.yml`, you first need to take `web.env.example` and fill it in with your details
and rename it to `web.env`. The options should be rather self explanatory.

## Running `docker-compose.yml`
To run it, simply do `docker-compose up`.

Be aware that once you do this, you should take extra care of the volumes created, of which one of them holds your
certificate and your private key!

## Explaining it all

`TODO`

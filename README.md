# Docker Nginx Reverse Proxy with Let's Encrypt Certificate Tutorial

This repository contains an example `docker-compose.yml` file to set up an nginx reverse proxy
with an auto-renewing [Let's Encrypt](https://letsencrypt.org/) TLS certificate for your web application.

<a name="pre-requisites"></a>
## Pre-requisites
 - [Docker](https://docs.docker.com/engine/installation/)
 - [Docker Compose](https://docs.docker.com/compose/install/) - optional -- to run the example
 - A working sub-domain
 - A working knowledge of Docker, and especially about
 [volumes](https://docs.docker.com/engine/tutorials/dockervolumes/)
 - Knowledge about how [Let's Encrypt](https://letsencrypt.org/) work

 <a name="submodule"></a>
## Submodule
When you clone this repository, don't forget to clone the submodule too!

```bash
git submodule init
git submodule update
```

<a name="config"></a>
## Configuration
To run the example `docker-compose.yml`, you first need to take `web.env.example` and fill it in with your details
and rename it to `web.env`.

`VIRTUAL_HOST` would be the host names, separated by a `,` for the list of host names nginx should listen to as
reverse proxy.

`LETSENCRYPT_HOST` would usually be the same as `VIRTUAL_HOST` and is the host name you want to retrieve a certificate
for. This host name *_must_* be publicly accessible. Enter your email address in `LETSENCRYPT_EMAIL`.

If you want to create test certificates that don't have the 5 certs/week/domain limits define the `LETSENCRYPT_TEST`
environment variable with a value of `true`.

<a name="run"></a>
## Running `docker-compose.yml`
To run it, simply do `docker-compose up`.

Be aware that once you do this, you should take extra care of the volumes created, of which one of them holds your
certificate and your private key!

<a name="wtf"></a>
## What is going on!?
Four containers are set up to orchestrate the entire affair. Namely:

 - Application Web Server (in our case, the `web` service which is just a simple Python HTTP server)
 - Nginx `nginx`
 - Nginx Reverse Proxy Configuration generator `nginx_gen`
 - Let's Encrypt certificate renwal and Nginx configuration generator `letsencrypt`

The first two containers should be fairly self explanatory.

The latter two containers make use of [`docker-gen`](https://github.com/jwilder/docker-gen) which generates nginx
configuration files based on the Docker environment.

The `nginx_gen` container listens on the Docker socket and writes configuration files for Nginx to act as the
reverse proxy for the containers with the `VIRTUAL_HOST` environment variable.

Similarly, the `letsencrypt` container will retrieve the Let's Encrypt certificate by performing the various
challenge steps that the ACME protocol requires by writing the appropriate files to the nginx HTML volume. Then,
it will use `docker_gen` to generate the appropriate configuration files for nginx.

<a name="run-independent"></a>
## Running the Nginx setup independently from other web applications
If you want to proxy multiple web applications with the same nginx setup, you should run the web application separately
from the nginx containers.

In this case, you can use `docker-compose.nginx.yml` as a starting point.

You would first have to create a network called `nginx` with `docker network create nginx` and make sure your
applications are also on the `nginx` network.

An example for how you might connect your application to the network can be found in `docker-compose.web.yml`

<a name="read-more"></a>
## Further Reading
 - [`docker_gen`](https://github.com/jwilder/docker-gen)
 - [`nginx_proxy`](https://github.com/jwilder/nginx-proxy)
 - [`docker-letsencrypt-nginx-proxy-companion`](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)

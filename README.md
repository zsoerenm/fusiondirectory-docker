# Fusiondirectory Docker

A composition of Docker containers to run Fusiondirectory.

## Features

- Use official containers if possible
- Follow best practices from [Docker](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

## Containers

- PHP: [zsoerenm/fusiondirectory-php](https://hub.docker.com/r/zsoerenm/fusiondirectory-php/) based on [php:7-fpm-stretch](https://hub.docker.com/_/php/)
- Nginx: [nginx:alpine](https://hub.docker.com/_/nginx/)
- Openldap: [zsoerenm/fusiondirectory-openldap](https://hub.docker.com/r/zsoerenm/fusiondirectory-openldap/) based on [osixia/openldap](https://hub.docker.com/r/osixia/openldap)

## Getting Started

## Usage

Simply run

```shell
docker-compose up
```
and head to `http://localhost/`.

## Install Fusiondirectory

If you visit fusiondirectory first, it will guide you through the installation procedure. During the installation procedure you will need to change files in the docker image. To do that exec into the running image with

```shell
docker-compose exec -it fusiondirectory bash
```
and do whatever you are advised to do.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

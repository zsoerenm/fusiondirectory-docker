# Fusiondirectory Docker

A composition of Docker containers to run Fusiondirectory.

By default it supports the following plugins: autofs,posix,quota,ldapdump,ldapmanager,mixedgroups,ssh,systems,mail,user-reminder,sudo
If you'd like to add or use other plugins, you will have to rebuild the images with the build argument PLUGIN_LIST that includes a comma seperated list of plugins.
Example:
```shell
docker build -t fusiondirectory-php --build-arg PLUGIN_LIST=autofs,quota ./fusiondirectory/
docker build -t fusiondirectory-openldap --build-arg PLUGIN_LIST=autofs,quota ./openldap/
```

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
docker exec -it <fusiondirectory_container> bash
```
and do whatever you are advised to do.

## License

MIT License

# Introduction

The `dev` [CLI](#cli) and [Docker images](#docker-images) provide a Docker powered local development experience for Laravel that is compatible with macOS, Windows (WSL2), and Linux. Other than Docker, no software or libraries need to be installed on your local machine.

# CLI

## Build

The `build` command can be used to build the Docker image of the services.

```bash
# build all services

dev build

# build only the `identity` and `matching` service

dev build identity matching
```

## Start

The `start` command can be used to run the Docker image of the services as a Docker container.

```bash
# start all services

dev start

# start only the `identity` and `matching` service

dev start identity matching
```

## Rebuild

The `rebuild` command can be used to rebuild the Docker image of the changed services and update their containers.

```bash
# rebuild all services

dev rebuild

# rebuild only the `identity` and `matching` service

dev rebuild identity matching
```

## Restart

The `restart` command can be used to restart the Docker container of the services.

```bash
# restart all services

dev restart

# restart only the `identity` and `matching` service

dev restart identity matching
```

## Stop

The `stop` command can be used to stop the Docker container of the services.

```bash
# stop all services

dev stop

# stop only the `identity` and `matching` service

dev stop identity matching
```

## Logs

The `logs` command can be used to print the logs of the Docker container of a service.

```bash
# print the logs of the `identity` service

dev logs identity

# print and follow the logs the `identity` service

dev logs identity -f
```

## Shell

The `shell` command can be used to open an interactive shell into the Docker container of a service.

```bash
# open an interactive shell into the `identity` service

dev shell identity
```

## Arbitrary commands

You can also run arbitrary commands in the Docker container of a service.

```bash
# install the composer dependencies of the `identity` service

dev identity composer install

# migrate the database of the `matching` service

dev matching php artisan migrate
```

# Docker images

...
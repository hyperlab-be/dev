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

## Exec

The `exec` command can be used to execute an arbitrary command in the Docker container of a service.

```bash
# install the composer dependencies of the `identity` service

dev exec identity composer install

# migrate the database of the `matching` service

dev exec matching php artisan migrate
```

# Docker images

## PHP

The PHP image can be used in a `Dockerfile` file as follows:

```dockerfile
FROM hyperlab-be/php-8.0

# set the timezone
ENV TZ=Europe/Brussels

# add a crontab to periodically run a command in the container
COPY scheduler /etc/cron.d/scheduler
# add a script to initialize the container every time it is started
COPY init /usr/local/entrypoints/init
# add a supervisor config file to run a daemon process in the container
COPY daemons.conf /etc/supervisor/conf.d/daemons.conf

ARG REPMAN_TOKEN

RUN echo \
    # install mysql
    && apt-get update \
    && apt-get install -y default-mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    # configure composer
    && composer config --global --auth http-basic.hyperlab.repo.repman.io token $REPMAN_TOKEN \
    # load the scheduler crontab
    && crontab /etc/cron.d/scheduler \
    # make the init script executable
    && chmod +x /usr/local/entrypoints/init
```

The other files should look something like this:

- `scheduler`:

    ```
    * * * * * /usr/local/bin/php /var/www/html/artisan schedule:run > /proc/1/fd/1 2> /proc/1/fd/2
    # An empty line is required at the end of this file for a valid cron file.
    ```

- `init`:

    ```
    #!/bin/bash

    php artisan route:cache
    php artisan config:cache
    php artisan pubsub:register
    php artisan migrate --force

    cron
    ```

- `daemons.conf`:

    ```
    [program:horizon]
    directory = /var/www/html
    command = php artisan horizon
    user=dev
    stdout_logfile=/dev/stdout
    stdout_logfile_maxbytes=0
    stderr_logfile=/dev/stderr
    stderr_logfile_maxbytes=0

    [program:pubsub]
    directory = /var/www/html
    command = php artisan queue:work pubsub
    user=dev
    stdout_logfile=/dev/stdout
    stdout_logfile_maxbytes=0
    stderr_logfile=/dev/stderr
    stderr_logfile_maxbytes=0
    ```

## Google Pub/Sub emulator

The Google Pub/Sub emulator image can be used in a `docker-compose.yml` file as follows:

```yaml
version: '3'
networks:
  dev: {}
volumes:
  pubsub: {}
services:
  pubsub:
    container_name: pubsub
    image: hyperlab-be/pubsub-emulator
    restart: unless-stopped
    ports:
      - 8085:8085
    networks:
      - dev
    volumes:
      - pubsub:/opt/data
    environment:
      - PUBSUB_PROJECT_ID=besomeone
```
#!/usr/bin/env bash

export DEV_USER=${DEV_USER:-$UID}
export DEV_GROUP=${DEV_GROUP:-$(id -g)}

if [ $# -gt 0 ]; then

    # dev build {?SERVICE}
    if [ "$1" == "build" ]; then
        docker compose build "${@:2}"

    # dev start {?SERVICE}
    elif [ "$1" == "start" ]; then
        docker compose up -d "${@:2}"

    # dev rebuild {?SERVICE}
    elif [ "$1" == "rebuild" ]; then
        docker compose up -d --build "${@:2}"

    # dev restart {?SERVICE}
    elif [ "$1" == "restart" ]; then
        docker compose restart "${@:2}"

    # dev stop {?SERVICE}
    elif [ "$1" == "stop" ]; then
        docker compose down "${@:2}"

    # dev logs {SERVICE}
    elif [ "$1" == "logs" ]; then
        docker compose logs "${@:2}"

    # dev shell {SERVICE}
    elif [ "$1" == "shell" ]; then
       docker compose exec -u dev "$2" bash

    # dev exec {SERVICE} {COMMAND}
    elif [ "$1" == "exec" ]; then
        docker compose exec -u dev "$2" "${@:3}"

    else
        echo "Unknown command"

    fi
    
else
    echo "Commands:" >&2
    echo
    echo "  build                       Build the Docker image of the services"
    echo "  build {service}             Build the Docker image of a service"
    echo
    echo "  start                       Run the Docker image of the services as a Docker container"
    echo "  start {service}             Run the Docker image of a service as a Docker container"
    echo
    echo "  rebuild                     Rebuild the Docker image of the changed services and update their Docker containers"
    echo "  rebuild {service}           Rebuild the Docker image of a changed service and update its Docker container"
    echo
    echo "  restart                     Restart the Docker container of the services"
    echo "  restart {service}           Restart the Docker container of a service"
    echo
    echo "  stop                        Stop the Docker container of the services"
    echo "  stop {service}              Stop the Docker container of a service"
    echo
    echo "  logs {service}              Print the logs of the Docker container of a service"
    echo
    echo "  shell {service}             Open an interactive shell into the Docker container of a service"
    echo
    echo "  exec {service} {command}    Execute an arbitrary command in the Docker container of a service"
    echo
    exit 1

fi
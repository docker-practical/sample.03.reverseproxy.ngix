# Introduce

sample development with docker

Create a Reverse Proxy with NGIX

Connect them with docker compose

following [guide](https://www.bogotobogo.com/DevOps/Docker/Docker-Compose-Nginx-Reverse-Proxy-Multiple-Containers.php)

## Description

### Dockerfile

```bash

FROM nginx:1.17

COPY nginx.conf /etc/nginx/nginx.conf //-> copy file ngix.config

// Or, COPY default.conf /etc/nginx/conf.d/default.conf

```

### Docker Compose file

```bash

version: '3'

services:
    reverse-proxy:
        build: ./ngix
        volumes:
            - ./ngix/nginx.conf:/etc/nginx/nginx.conf
        ports:
            - 5001:5001 //-> map port 5001 on real-machine to 5001 on proxy-container, proxy of website1 listen on 5001
            - 5002:5002 // the same for website2
        restart: unless-stopped
    website1:
        image: php:apache
        volumes:
            - ./website1:/var/www/html
        expose:
            - 80
        depends_on:
            - reverse-proxy
    website2:
        image: php:apache
        volumes:
            - ./website2:/var/www/html
        expose:
            - 80
        depends_on:
            - reverse-proxy

```

## Usage

### Run compose

```bash

docker-compose up

```

### Run compose with -d

To run container with demount option, container will run on background

```bash

docker-compose up -d

docker-compose stop

```
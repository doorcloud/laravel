# Launching a Laravel (PHP) Application with Docker

This guide explains how to set up and launch a Laravel (PHP) application with Apache using Docker.

## Prerequisites

Before starting, ensure you have the following tools installed on your machine:

- [Docker](https://www.docker.com/products/docker-desktop)
- [Composer](https://getcomposer.org/)
- [Laravel](https://laravel.com/docs/)

## Dockerfile Content

This Dockerfile configures a container for a Laravel (PHP) application with Apache. It installs the PHP `gd` extension, configures PHP to use the production environment, and installs PHP dependencies via Composer.

```Dockerfile
ARG PHP_EXTENSIONS="gd"

FROM thecodingmachine/php:8.3-v4-apache

ENV PHP_EXTENSION_GD=1
ENV TEMPLATE_PHP_INI=production

COPY .env.example .env
COPY --chown=docker:docker . /var/www/html

ENV APACHE_DOCUMENT_ROOT=public/

RUN composer install
RUN php artisan key:generate

```
## Steps to Launch the Application

1. Build the Docker Image

To build the Docker image, use the following command in the directory containing the Dockerfile:

```
docker build -t door-laravel .
```

2. Run the Container

Once the image is built, run a container from this image:

```
docker run -p 8080:80 door-laravel
```

3. Access the Application

Open your browser and go to the following URL to see your application running:

```
http://localhost:8080
```

4. Environment Variables

If you need to configure additional environment variables, modify the .env file that was copied into the container during the image build.

## Publishing the Image on Docker Hub

1. Log In to Docker Hub

Before publishing your image, log in to Docker Hub with your Docker account:

```
docker login
```

2. Tag the Image

Tag the image you built with your Docker Hub username and the image name:

```
docker tag door-laravel your_dockerhub_username/door-laravel:latest
```
Replace your_dockerhub_username with your Docker Hub username.

3. Push the Image to Docker Hub

Push the tagged image to Docker Hub:

```
docker push your_dockerhub_username/door-laravel:latest
```



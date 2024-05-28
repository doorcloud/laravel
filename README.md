# Lancement de l'Application Laravel (PHP) avec Docker

Ce guide vous explique comment configurer et lancer une application PHP avec Apache en utilisant Docker.

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les outils suivants sur votre machine :

- [Docker](https://www.docker.com/products/docker-desktop)
- [Composer](https://getcomposer.org/)
- [Laravel](https://laravel.com/docs/)

## Contenu du Dockerfile

Ce Dockerfile configure un conteneur d'une application Laravel(PHP) avec Apache. Il installe l'extension PHP `gd`, configure PHP pour utiliser l'environnement de production, et installe les dépendances PHP via Composer.

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
## Étapes pour lancer l'application

1. Construire l'image Docker

Pour construire l'image Docker, utilisez la commande suivante dans le répertoire contenant le Dockerfile :

```
docker build -t door-laravel .
```

2. Lancer le conteneur

Une fois l'image construite, lancez un conteneur à partir de cette image :

```
docker run -p 8080:80 door-laravel
```

3. Accéder à l'application

Ouvrez votre navigateur et accédez à l'URL suivante pour voir votre application en cours d'exécution :

```
http://localhost:8080
```

4. Variables d'environnement

Si vous devez configurer des variables d'environnement supplémentaires, modifiez le fichier .env qui a été copié dans le conteneur lors de la construction de l'image.

## Publier l'image sur Docker Hub

1. Se connecter à Docker Hub

Avant de publier votre image, connectez-vous à Docker Hub avec votre compte Docker :

```
docker login
```

2. Taguer l'image

Taguez l'image que vous avez construite avec votre nom d'utilisateur Docker Hub et le nom de l'image :

```
docker tag door-laravel your_dockerhub_username/door-laravel:latest
```
Remplacez your_dockerhub_username par votre nom d'utilisateur Docker Hub.

3. Pousser l'image sur Docker Hub

Poussez l'image taguée sur Docker Hub :

```
docker push your_dockerhub_username/door-laravel:latest
```



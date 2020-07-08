<p align="center"><img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400"></p>

## Laravel install via Composer
```shell
$ cd <ProjectFolder>
$ docker run --rm --interactive --tty --volume $(PWD):/app composer:1.9.3 -v
$ docker run --rm --interactive --tty --volume $(PWD):/app composer:1.9.3 create-project --prefer-dist laravel/laravel laravel
$ cd laravel
```

volumes directive tells docker to mount the host's app source code into /var/www/html - which is consistent with apache configuration. This enables any change from host files be reflected in the container. Commands such as composer require will add vendor to host, so we won't need to install dependencies everytime container is brought down and up again.

If you are building container for CI / remote VM envrionment however, you'll need to add the source files into the container pre-build.

## Laravel Application Settings
```shell
$ cp ./laravel/.env.example ./laravel/.env
$ vi ./laravel/.env
```

```shell
1c1
< APP_NAME=Laravel
---
> APP_NAME=Revenue
3c3
< APP_KEY=
---
> APP_KEY=base64:IdfzyUNaJmfiShpXMO3Gghm7Jy3mSEoYxmWhH2BBm3g=
5c5
< APP_URL=http://localhost
---
> APP_URL=http://localhost:8095
10c10
< DB_HOST=127.0.0.1
---
> DB_HOST=db
13,14c13,14
< DB_USERNAME=root
< DB_PASSWORD=
---
> DB_USERNAME=laravel
> DB_PASSWORD=password
```

## Docker Settings
```shell
$ cp .env.example .env
$ vi .env
```

## If necessary Create a Docker Network
```shell
$ docker network create webproxy
```

## Create Docker Containers
```shell
$ docker-compose up -d
```

## Laravel Install (not mandatory when create-project is executed)
```shell
$ docker exec <app_container_name> composer install
```

## Laravel Nova download
Go check https://nova.laravel.com/releases  
Download appropriate release (for instance nova-3.6.0.zip)

```shell
$ cp nova-3.6.0.zip /tmp/
$ cd /tmp/
$ unzip nova-3.6.0.zip
$ mv laravel-nova-5f298f21c7f214a9b22dc6ec900d2b6439377a09 <ProjectFolder>/laravel/nova

$ cd <ProjectFolder>/laravel
$ vi composer.json
```

```json
"repositories": [
    {
        "type": "path",
        "url": "./nova"
    }
],
```

```json
"require": {
    "php": "^7.2.5",
    "fideloper/proxy": "^4.2",
    "laravel/framework": "^7.0",
    "laravel/nova": "*"
},
```

## Laravel Nova Install
```shell
$ docker exec <app_container_name> composer update
$ docker exec <app_container_name> php artisan nova:install
$ docker exec <app_container_name> php artisan migrate
```

## Create First Nova Admin
```shell
$ php artisan nova:user
```

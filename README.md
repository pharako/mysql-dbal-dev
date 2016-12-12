MySQL DBAL Dev
==============

Development environment for [MySQL DBAL](https://github.com/pharako/mysql-dbal).

# Overview

This repository contains everything you need to test [MySQL DBAL](https://github.com/pharako/mysql-dbal). It consists of two interconnected Docker containers, one running PHP and another running either MySQL or MariaDB.

# Requirements

Docker Engine 1.13 or above.

## Download MySQL DBAL

Place a copy of [MySQL DBAL](https://github.com/pharako/mysql-dbal) under the [`repos`](repos) directory. You can use any cloning strategy you may like, just make sure the clone's path is `repos/mysql-dbal`.

## Build the custom PHP image

The following command line will use the Dockerfile under [`docker/php`](docker/php/Dockerfile) to build a custom PHP image with Composer already installed:

```SHELL
$ docker-compose build php
```

Database images are pulled from Dockerhub, so there's no need to build them yourself.

## Run the database container

You can run the tests either against MySQL or MariaDB.

### MySQL

The following command line will pull the MySQL image from Dockerhub, if necessary, and spin up a new container based on it:

```SHELL
$ docker-compose up -d db
```

### MariaDB

The following command line will pull the MariaDB image from Dockerhub, if necessary, and spin up a new container based on it:

```SHELL
$ docker-compose -f docker-compose.yml -f docker-compose.mariadb.yml up -d db
```

### Check the database status

To check if the container is running and the database configurations are correctly set up, you can use

```SHELL
$ docker exec -ti mysql-dbal-db mysql -u testdb -ptestdb testdb -e 'show databases;'
```

## Run the tests from inside the PHP container

Spin up a new PHP container:

```SHELL
$ docker-compose run --rm php
```

From inside that container, install the Composer packages:

```SHELL
$ composer install
```

You can then run the tests:

```SHELL
$ DB_HOST=mysql-dbal-db \
DB_DATABASE=testdb \
DB_USERNAME=testdb \
DB_PASSWORD=testdb \
./vendor/bin/codecept run unit
```

## Bring everything down

To delete all containers, run

```SHELL
$ docker-compose down
```


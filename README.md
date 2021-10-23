# Symfony DDD CQRS

This project is sample stack Domain Driven Design (DDD)  and CQRS with Symfony 5 into Docker containers using Docker-compose,
using MySQL 8 as a database.


![DDD](img/ddd.png?raw=true "DDD")

## Installation

1. Run `docker-compose up -d`

```
 ⠿ Network symfony-ddd-cqrs_default                 Created                                                        0.0s
 ⠿ Container symfony-ddd-cqrs-elasticsearch-1       Started                                                        1.1s
 ⠿ Container symfony-ddd-cqrs-rmq-1                 Started                                                        1.1s
 ⠿ Container symfony-ddd-cqrs-mysql-1               Started                                                        0.8s
 ⠿ Container symfony-ddd-cqrs-code-1                Started                                                        0.8s
 ⠿ Container symfony-ddd-cqrs-start_dependencies-1  Started                                                        2.0s
 ⠿ Container symfony-ddd-cqrs-workers_users-1       Started                                                        2.1s
 ⠿ Container symfony-ddd-cqrs-php-1                 Started                                                        2.2s
 ⠿ Container symfony-ddd-cqrs-workers_events-1      Started                                                        2.0s
 ⠿ Container symfony-ddd-cqrs-nginx-1               Started                                                        3.1s
```

2. Use this value for the DATABASE_URL environment variable of Symfony:

```
DATABASE_URL=mysql://root:api@mysql:3306/api?serverVersion=8.0
```

User and password of the database is the `env` file at the root of the project.

3. In your dev environment add this line in your hosts file:
```
127.0.0.1 dev.app.com
```

4.- Execute composer install
```
docker exec -ti  symfony-ddd-cqrs-code-1 composer install
```

5.- Execute migrations

View status:
```
docker exec -ti symfony-ddd-cqrs-code-1 php bin/console doctrine:migrations:status
```
execute all migrations pending:
```
docker exec -ti symfony-ddd-cqrs-code-1 php bin/console doctrine:migrations:migrate 
```

Default path for bundle:
```
http://dev.app.com/user/
http://dev.app.com/order/
```

6.- Tools for testing, fixed and evalute code.

For execute PHPUnit, PHPStan, Psalm and PHP-cs-fixer in development time

- for execute PHPUnit
```
docker exec -ti symfony-ddd-cqrs-code-1 php vendor/bin/phpunit
```
- execute PHPStan
```
docker exec -ti symfony-ddd-cqrs-code-1 php vendor/bin/phpstan
```
- execute Psalm
```
docker exec -ti symfony-ddd-cqrs-code-1 php vendor/bin/psalm --show-info=true
```
- execute PHP-cs-fixer
```
docker exec -ti symfony-ddd-cqrs-code-1 php vendor/bin/php-cs-fixer fix src
```

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-5-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

A boilerplate for DDD, CQRS, Event Sourcing applications using Symfony as framework and running with PHP 8.

![push](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/workflows/push/badge.svg)
[![Coverage Status](https://coveralls.io/repos/github/jorge07/symfony-5-es-cqrs-boilerplate/badge.svg?branch=symfony-5)](https://coveralls.io/github/jorge07/symfony-5-es-cqrs-boilerplate)

> Symfony 4 still available in [symfony-4 branch](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/tree/symfony-4)

## Documentation

[Buses](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/tree/master/doc/GetStarted/Buses.md)

[Creating an Application Use Case](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/tree/master/doc/GetStarted/UseCases.md)

[Adding Projections](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/tree/master/doc/GetStarted/Projections.md)

[Async executions](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/tree/master/doc/GetStarted/Async.md)

[UI workflow](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/blob/master/doc/Workflow.md)

[Xdebug configuration](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/blob/master/doc/GetStarted/Xdebug.md)

[Kubernetes Deployment](https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/blob/master/doc/Deployment.md)

## Architecture

![Architecture](https://i.imgur.com/SzHgMft.png)

## Implementations

- [x] Environment in Docker
- [x] Symfony Messenger
- [x] Event Store
- [x] Read Model
- [x] Async Event subscribers
- [x] Rest API
- [x] Web UI (A Terrible UX/UI)
- [x] Event Store Rest API 
- [x] Swagger API Doc

## Use Cases

#### User
- [x] Sign up
- [x] Change Email
- [x] Sign in
- [x] Logout

![API Doc](https://i.imgur.com/DBZsPlE.png)

## Stack

- PHP 8+
- Mysql 8.0
- Elastic & Kibana 7.11.0
- RabbitMQ 3

## Project Setup


|    Action        	|     Command    |
|------------------	|---------------	|
|  Setup 	          | `make start`   |
|  Run Tests       	| `make phpunit` |
|  Static Analisys 	| `make style`  	|
|  Code Style      	| `make cs`     	|
|  Code style check	| `make cs-check`|
|  PHP Shell 	      | `make s=php sh`|
|  Xdebug 	         | `make xoff/xon`|
|  Build Artifacts   | `make artifact`|

## PHPStorm integration

PHPSTORM has native integration with Docker compose. That's nice but will stop your php container after run the test scenario. That's not nice when using fpm. A solution could be use another container just for that purpose but is way slower and I don't want. For that reason I use ssh connection.

### IMPORTANT

> **ssh in the container it's ONLY for that reason and ONLY in the DEV TAG, if you've ssh installed in your production container, you're doing it wrong...***

[Click here for the detailed instructions about how to setup the PHP remote interpreter in PHPStorm.](https://github.com/jorge07/alpine-php/blob/master/doc/IDE.md)

If you're already familiar with it, here a quick configuration reference:

|    Host          	|    Direction  |
|------------------	|--------------	|
|  Docker 4 mac 	   | `localhost`   |
|  Dinghy       	   | `$ dinghy ip` |

**Port:** `2323`

**Filesystem mapping:** `{PROJECT_PATH}` -> `/app`

### Xdebug

To ease your development process, you can use Xdebug with PHPSTORM.

1. Add a Docker interpreter

   ![Docker PHP interpreter](doc/docker-php-interpreter.png)

2. Enable Xdebug listenning. Don't forget to also activate Xdebug helper from your browser.
   
   ![Xdebug activation](doc/xdebug-activation.png)
   
   Additionally, you can check `Break at first line in PHP scripts` to ensure your debug is working.

3. Make a request from you API at http://127.0.0.1/api/doc for example. You should see this popup:

   ![Xdebug mapping](doc/xdebug-mapping.png)
   
   Click on `Accept` and you should be ready to debug ! Start placing breakpoints on your code and enjoy debugging !

> Note for Windows users:
>
> You might need to update `docker-os=` to `docker-os=windows` in [Makefile](makefile)
> or specify its value on command line like `$ make start docker-os=windows`.

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="http://tacon.eu"><img src="https://avatars2.githubusercontent.com/u/2017676?v=4" width="100px;" alt=""/><br /><sub><b>Luis</b></sub></a><br /><a href="https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/commits?author=Lutacon" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/cv65kr"><img src="https://avatars0.githubusercontent.com/u/9404962?v=4" width="100px;" alt=""/><br /><sub><b>Kajetan</b></sub></a><br /><a href="https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/commits?author=cv65kr" title="Code">💻</a></td>
    <td align="center"><a href="https://coderslab.pl"><img src="https://avatars0.githubusercontent.com/u/2781079?v=4" width="100px;" alt=""/><br /><sub><b>Krzysztof Kowalski</b></sub></a><br /><a href="https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/commits?author=kowalk" title="Code">💻</a></td>
    <td align="center"><a href="http://patryk.it"><img src="https://avatars3.githubusercontent.com/u/17459288?v=4" width="100px;" alt=""/><br /><sub><b>Patryk Woziński</b></sub></a><br /><a href="https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/commits?author=patrykwozinski" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/jon-ht"><img src="https://avatars3.githubusercontent.com/u/17051512?v=4" width="100px;" alt=""/><br /><sub><b>jon-ht</b></sub></a><br /><a href="https://github.com/jorge07/symfony-5-es-cqrs-boilerplate/commits?author=jon-ht" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

### The task

- build Docker image named php-composer based on PHP 7.2.3
- run Composer install using this image with next requirements:
  - mount PHP project's root to the container using `volume`
  - container must be started under local user's UID so data installed to the `vendor` directory will be owned by same UID/GID
  - example command to run: `docker run ... php-composer composer install`
- add manual for Linux OS to use this image

### The solution

The `Dockerfile` in this project uses multi-stage Docker builds to create a new image with Composer (latest) and PHP 7.2.3.
You can build an example project, which will be owned by some _localuser_'s UID/GID, using `composer.json` included in this repository with following steps:   

1. Build Docker image using [Dockerfile](https://github.com/setevoy2/docker-php-composer/blob/master/Dockerfile) from this repository: `docker build -t php-composer .`  
2. Build project using `--user` and `--volume` options for `docker run`: `docker run --user $(id -u localuser):$(id -g localuser) -ti --volume $(pwd)/:/app php-composer composer install` 

You can find much more details in blog post [Docker: PHP Composer и multi-stage билды Docker образов
](https://rtfm.co.ua/docker-php-composer-i-multi-stage-bildy-docker-obrazov/) (in Russian only for now).

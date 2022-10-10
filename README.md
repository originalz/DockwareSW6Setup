

## Dockware Installation

### Basic Setup
  - Create new project dir and docker-compose.yml with dockware/dev:[VERSION] image -> WSL
  - Execute docker compose up -d -> WSL

### Directory Setup
  - /src/custom/plugins/[PLUGINS_FOR_DEV] -> WSL
  - /var/www/html/custom/plugins/[PLUGINS_FOR_DEV] -> DOCKER
  - /var/www/html/.env -> DOCKER

### Bind Mounting
 - docker compose stop -> WSL
 - Add volumes to docker-compose.yml -> WSL
 - chgrp -R 33 ./src -> WSL
 - chmod a+w ./src/var/* -> WSL
 - docker compose up -d -> WSL
 - sudo chown www-data:www-data /var/www/html -R -> DOCKER
 - Test if data is synced between docker and WSL project files
 - move to project dir and execute pst ps to start PHPStorm with the WSL dir -> WSL
  
### Fix very rare [Dockware Error](https://docs.dockware.io/faq/mysql-failed) 
 - docker rm -f [CONTAINERNAME] -> WSL

## Docker

### Handy Docker commands
  - docker exec [-it] [CONTAINER_NAME] bash -> get acces to the docker machine
  - docker compose up [-d] -> start docker machine
  - docker compose stop -> stop docker machine
  - docker compose down -> delete docker machine
  - docker ps [-a] -> show all available docker machines



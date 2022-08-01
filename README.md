## Dockware Installation

### Basic Setup
  - Create new project dir and docker-compose.yml with dockware/dev:latest image without volumes attributes at first [WSL]
  - Execute docker compose up -d to init the docker container [WSL]
  - Create src/custom/plugins dir for the project data [WSL]

### Directory Setup
  - /src/custom/plugins/<PLUGINS FOR DEVELOPMENT> [Plugins WSL]
  - /var/www/html/custom/plugins/<PLUGINS FOR DEVELOPMENT> [Plugins DOCKER]
  - /var/www/html/.env [.env DOCKER]

### Bind Mounting
  - docker compose stop [WSL]
  - Add volumes to docker-compose.yml [WSL]
  - chgrp -R 33 ./src [WSL]
  - chmod a+w ./src/var/* [WSL]
  - docker compose up -d [WSL]
  - sudo chown www-data:www-data /var/www/html -R [DOCKER]
  - Test if data is synced between docker and WSL project files
  - move to project dir and execute pst ps to start PHPStorm with the WSL dir [WSL]

## Docker

### Handy Docker commands
  - docker exec <-it> CONTAINER_NAME bash
  - docker compose up <-d>
  - docker compose stop
  - docker compose down
  - docker ps <-a>

## Shopware 6 Basics
  
### Repositories
```php
  $criteria = new Criteria();
  $criteria->addFilter(new EqualsAnyFilter('<DATA>', [<DATA_ARRAY>]));
  $criteria->addAssociation('field');
  $criteria->addAssociation('field.attribute');
  /** @var Entity $entity */
  $entity = EntityRepositoryInterface $repository->search($criteria, $this->context);
```

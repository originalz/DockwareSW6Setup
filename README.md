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

### Setup Custom Fields
Initialize Custom Field on Install

```php
public function install(InstallContext $installContext): void
{
	/** @var EntityRepository $customFieldSetRepository */
	$customFieldSetRepository = $this->container->get('custom_field_set.repository');

	$customFieldSetRepository->upsert([
		[
			'name' => '<prefix_custom_field_set>',
			'config' => [
				'label' => [
					'de-DE' => '<LABEL>',
					'en-GB' => '<LABEL>'
				]
			],
			'customFields' => [
				[
					'name' => '<prefix_customfield_name>',
					'type' => CustomFieldTypes::<TYPE>,
					'config' => [
						'label' => [
							'de-DE' => '<LABEL>',
							'en-GB' => '<LABEL>'
						],
						'type' => '<TYPE>',
						'customFieldType' => '<TYPE>',
						'customFieldPosition' => 1
					]
				]
			],
			'relations' => [
				[
					'id' => Uuid::randomHex(),
					'entityName' => '<ENTITY_NAME>'
				]
			]
		]
	], $installContext->getContext());
}
```

Delete Custom Field on Uninstall

```php
public function uninstall(UninstallContext $uninstallContext): void
{
	$connection = $this->container->get(Connection::class);

	$connection->executeUpdate('
		DELETE FROM `custom_field_set`
		WHERE name LIKE \'<prefix_custom_field_set>\'
	');

	$connection->executeUpdate('
		DELETE FROM `custom_field`
		WHERE name LIKE \'<prefix_customfield_name>\'
	');
}
```

Fill Custom Field
```php
public function setCustomField(string $param): void  
{  
	$criteria = new Criteria();  
	$criteria->addFilter(new EqualsAnyFilter('<IDENTIFIER>', [$param]));  
  
	/** @var Entity $entity */  
	$entity = $this->repository->search($criteria, $this->context)->first();  
  
	$this->repository->update([  
		[  
			'id' => $entity->getId(),  
			'customFields' => ['<prefix_customfield_name>' => '<data>']  
		]
	], $this->context);  
}
```

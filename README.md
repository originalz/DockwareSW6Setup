# Dockware Shopware 6 Environment

## Basic Setup
The basic setup gives you a bind mounted shopware 6 installation with custom dockware image, shopware version & PHP version. If you need different versions for your project, just change it in the `docker-compose.yml` and run the `docker compose up...` command

 1. Create a new directory for your project on your machine.
 2. Create a docker-compose.yml file for your Dockware setup to define a docker container.
 3. Execute `docker compose up -d` to run the container.
 4. Execute `docker exec -it <CONTAINERNAME> bash` to get access to the docker container
 5. Execute `sudo chown www-data:www-data /var/www/html -R` to fix rights on the machine

## Docker Compose File
    version: "3"
    
    services:
    
	    shopware:
		    image: dockware/dev:latest # Choose Dockware Image and SW Version
		    environment:
			    - PHP_VERSION=8.0 # Choose PHP Version
		    container_name: SW6_NAME # Choose container name
		    ports:
				- "80:80"
			    - "22:22"
		    volumes:
			    - "./shop/custom/plugins:/var/www/html/custom/plugins" # Setup volume for mounting
		    networks:
			    - web

    networks:
	    web:
		    external: false

## Fix very rare MySQL error

 - [MySQL failed on startup](https://docs.dockware.io/faq/mysql-failed)
 
## PHPStorm Utility

 - PHPStormTools PHPStorm Path: "C:\Users\keanu.klenner\AppData\Roaming\PHPStormTools"
 - ZSH PHPStormTools Alias for pst "~/.zshrc"

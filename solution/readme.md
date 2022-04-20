Initial Setup and Updating Containers
=====================================

## Initial setup

Setup consists of three Docker containers: Django application, Postgres database and Traefik reverse-proxy. Current configuration of docker-compose builds image for Django and pulls images for Traefik and Postgres. Setup automatically checks if database ready for connections before running Django. After deploying first time Django application needs to be configured: performing initial migrations and setting up superuser. Database volume should be created prior to executing compose file.  

## Updating Containers

Depending on final configuration of compose-file (development or production) update pattern has slight differences. Also while containers are recreataed, certain actions might be required: performing migrations after models modifications, checking database configuration, etc.

---

### Development

Development configuration (which is present in this repository) of docker-compose that might be used for testing purposes and in early stages of application's life-cycle, uses "hot-reload" of Django source code and builds it's container. Containers update includes: pulling newer versions of images, building modified images, restarting containers:

1. `docker-compose pull <service_name>` - will pull newer versions of image for service without interfearing container work. 
2. `docker-compose up --force-recreate --build -d <service_name>` - will recreate container using newly-pulled image and build images that require building.
3. `docker image prune -f` - will delete old images. 

### Production 

In production configuration changes mainly concern Django application: it should be pre-built with previous and new migrations included and stored in container repository. Also application would be run with gunicorn instead of built-in server and as result "hot-reload" of source code won't be used. In this case containers update includes: only pulling newer versions of images, restarting containers:

1. `docker-compose pull <service_name>` - will pull newer versions of image for service without interfearing container work. 
2. `docker-compose up --force-recreate -d <service_name>` - will recreate container using newly-pulled image.
3. `docker image prune -f` - will delete old images.

Also, assuming that everything is thoroughly tested we could prepend `python manage.py makemigrations && python manage.py migrate` to running script, so that all changes to database would be in place when application launches.

### Running commands

There might be a need for executing commands in running containers, which is achievable with this command:

- `docker-compose exec <service_name> <command>` - used for running commands in container's CLI (migrate model changes, running django shell, postgres psql etc.)

## Additional info

---

### About Django application

Application used for challange is a first few steps of Django tutorial from official website, which has minimal functionality required to test database interaction and configuration. Can be tested throgh admin panel.


### Adding Django instances

To increase failover by adding additional instance of Django application use next command:

`docker-compose scale polls=<number_of_instances>`

It will create additional containers, which will be automatically discovered by Traefik and load will be balanced between all new instances.
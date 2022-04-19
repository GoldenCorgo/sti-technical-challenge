Initial Setup and Updating Containers
=====================================

## Initial setup

Setup consists of three Docker containers: Django application, Postgres database and Traefik reverse-proxy. Current configuration of docker-compose builds image for Django and pulls images for Traefik and Postgres. Setup automatically checks if database ready for connections before running Django. After deploying first time Django application needs to be configured: performing migrations and setting up superuser.  

## Updating Containers

Depending on final configuration of compose-file (development or production) update pattern has slight differences. Also after containers are recretaed, certain actions might be required: performing migrations after models modifications.

---

### Development

Development configuration (which is present in this repository) of docker-compose that might be used for testing purposes and in early stages of application's life-cycle, uses "hot-reload" of Django source code and builds it's container. Containers update includes: pulling newer versions of images, pre-building modified images, restarting containers:

1. `docker-compose build <service_name>` - will build newer versions of images for service without interfearing container work.
2. `docker-compose pull <service_name>` - will pull newer versions of image for service without interfearing container work. 
3. `docker-compose up --force-recreate -d` - will recreate container using newly-pulled image.
4. `docker image prune -f` - will delete old images.

### Production 

In production configuration changes mainly concern Django application: it should be pre-built with previous and new migrations included and stored in container repository. Also application would be run with gunicorn instead of built-in server and as result "hot-reload" of source code won't be used. In this case containers update includes: only pulling newer versions of images, restarting containers:

1. `docker-compose pull <service_name>` - will pull newer versions of image for service without interfearing container work. 
2. `docker-compose up --force-recreate -d` - will recreate container using newly-pulled image.
3. `docker image prune -f` - will delete old images.

### Post-update actions

- Container-specific actions:
    - `docker-compose exec <service_name> <command>` - used for running commands in container's CLI (migrate model changes, running django shell, etc.)
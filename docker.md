# Docker

## Terms
- _Image_
  - A template for containers that describes a snapshot of a system (including all environment) at a particular time.
  - includes OS, software, and application code.
- _Container_
  - A running instance of an image.
- _Dockerfile_
  - Describes an image.
  - A textfile that defines all the dependencies for the image.
- _Docker Hub_
  - A repository of public (open-source) Dockerfiles.
  - Search here if you want e.g. "Apache running on Debian".
- _Volume_
  - Mount a file or directory into the container.
  - This allows you to dynamically change application code without having to re-run a new container everytime your image changes.
  - By default, without volumes, a Docker image takes a snapshot of your application code and runs it in the container - any changes you make after that will not be reflected in your locally-running app (until you )
- _Docker compose_
  - manages multiple-container applications
  - allows you to run multiple containers (from multiple images) with a single command


You write a _Dockerfile_, which you build to produce an _image_, and that image is run to create a _container_.

## Questions
- Is Docker really much better than using dependency managers etc.
- If I'm developing a web app on a Mac, and I'm using a Docker image that's running Ubuntu and Apache, does that mean that my local app is "actually" running on Ubuntu?
  - Does your production environment use the same image as your development environment?

- Difference between Docker containers and virtual machines (VMs)?
- Why is it better to have one process per container?
  - And what exactly do we mean by that?

## Advantages
- Same environment
  - app always runs in the same environment, so you never have to debug "works on my machine but not hers".
- Sandbox projects
  - different projects on your machine are separate and don't have shared dependencies.
- It Just Works
  - Don't have to install a million dependencies before you start a new project.
  
## Dockerfile example
- Images can be layered on top of each other. Eg your image relies on an Apache image which relies on a Debian image.

```
# FROM calls the `php` image, passing a specific tag showing which version of php (7.0) and to get apache too.
FROM php:7.0-apache

# Tell the image where apache should expect your app code (in `src/`).
COPY src/ /var/www/html

# The container will listen to incoming requests on port 80
EXPOSE 80
```

## Usage
- `build`: Create an image from a Dockerfile
  - this downloads all the dependencies from the various image layers.

- `run`: Create a container from an image

## Docker Compose
- Useful when your web app has a microservices architecture, and you need to run multiple containers at once, each representing a different microservice.

- Create a `docker-compose.yml` file.
  - This specifies the different processes that comprise your application, each with their own image.
- `docker-compose up`: Downloads all necessary images and runs their containers.
  
## Sources
- [Learn Docker in 12 minutes](https://www.youtube.com/watch?v=YFl2mCHdv24&ab_channel=JakeWright)


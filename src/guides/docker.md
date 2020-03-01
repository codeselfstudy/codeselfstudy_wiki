# Docker Guide

At the moment, this Docker guide focuses on tips that can help while working on the [Code Self Study website](https://github.com/codeselfstudy/codeselfstudy). To learn the basics of Docker, it might be a good idea to start with [the official Docker guides](https://docs.docker.com/get-started/).

## Basic Concepts

A Docker container is something like a virtual computer that runs inside of your computer. Instead of installing software (like a database) on your computer directly, you can put it in its own container and run it from there. Multiple containers can be networked with each other to communicate. An end result of using Docker is that all the programmers on a project have the same development environment, including the same versions of all the required software.

A **Docker image** is a kind of blueprint for a container.

**Docker containers** are created from the images. You can have multiple docker containers that are based on the same image.

A **Docker network** connects multiple containers. If you use **Docker Compose** to manage your containers, it will automatically create the network for you.

A **Docker volume** mounts files and persists data after a Docker container is destroyed.

The base Docker command is `docker`. Other parts of Docker have sub-commands. For example, the commands for managing containers are behind `docker container`, the commands for managing images are behind `docker image`, and the commands for managing volumes are behind `docker volume`.

To list your Docker images, you would use:

```text
$ docker image ls
```

To list your Docker containers, you would use:

```text
$ docker container ps
```

The built-in help is good -- just type `docker container` to see the options for that sub-command.

You can download pre-made Docker images with the `docker pull` command, but the Code Self Study website creates custom images by using Docker Compose and Dockerfiles. If you look inside the `Dockerfile.dev` files in the project, you can see the commands that are run.

For example, the `Dockerfile.dev` file for the Express.js server looks something like this:

```text
FROM node:12-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

It says:

1. Start with a preexisting Node.js image from [Docker Hub](https://hub.docker.com/) (like Github for Docker images), using Node version 12 and Alpine Linux (a very small Linux distro).
1. Set the working directory inside the images to `/app`.
1. Copy the `package.json` file(s) to the working directory.
1. Install the dependencies.
1. Copy the Express.js code into the working directory.
1. Suggest exposing port 5000 for the Express server.
1. Start the Express server with `npm start`.

A `Dockerfile` can be built manually, but in the case of Code Self Study, all the containers are built with the `docker-compose.yml` file. Take a look [in that file](https://github.com/codeselfstudy/codeselfstudy/blob/master/docker-compose.yml) to see how the `Dockerfiles` are used.

```yaml
# The API for the puzzle server
express_api:
    # wait for other containers to start first
    depends_on:
        - mongo
        - redis
    # build from the ./containers/express_api/Dockerfile.dev file
    build:
        dockerfile: Dockerfile.dev
        context: ./containers/express_api
    # mount the project files inside the container
    volumes:
        - /app/node_modules
        - ./containers/express_api:/app
    # set some environment variables
    environment:
        - REDIS_HOST=redis
        - REDIS_PORT=6379
        - SESSION_SECRET="this is only for development"
```

## Restarting Containers

Rebuilding the entire application can take a while, and sometimes you will just want to restart one container (like the Gatsby container).

Here's how to restart a Docker container.

First, get a list of running containers:

```text
$ docker container ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
f9466971430a        codeselfstudy_nginx         "nginx -g 'daemon of…"   6 minutes ago       Up 6 minutes        0.0.0.0:4444->80/tcp   codeselfstudy_nginx_1
b9b10f71037f        codeselfstudy_gatsby        "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes                               codeselfstudy_gatsby_1
18febd3d8b84        codeselfstudy_express_api   "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes        5000/tcp               codeselfstudy_express_api_1
d2d54e20f529        mongo                       "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes        27017/tcp              codeselfstudy_mongo_1
75df2bbce79f        redis:latest                "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes        6379/tcp               codeselfstudy_redis_1
```

If the container that you want to restart is `codeselfstudy_gatsby_1`, then copy the container ID and restart it like this:

```text
$ docker container restart b9b10f71037f
```

You can also refer to it by the full name, if that's easier:

```text
$ docker container restart codeselfstudy_gatsby_1
```

## Freeing Disk Space

After a while, it's possible that Docker images will take up a lot of hard drive space. It's easy to clean them.

To list the images use this command:

```text
$ docker image ls

REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
<none>                      <none>              7d3aa3d696c0        2 minutes ago       307MB
codeselfstudy_express_api   latest              c7a0d49e0ade        2 minutes ago       140MB
```

To remove an image, for example the unnamed image above, refer to it by the `IMAGE ID`:

```text
$ docker image rm 7d3aa3d696c0
```

You can list the dangling images (untagged as `<none>`) with this command:

```text
$ docker image ls -f "dangling=true" -q
```

And delete them with this command:

```text
$ docker image rm $(docker image ls -f "dangling=true" -q)
```

Or just use this command to remove the dangling images, stopped containers, and reclaim other space:

```text
$ docker system prune
```

To see how much space Docker is using type this:

```text
$ docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              28                  5                   14.27GB             13.53GB (94%)
Containers          5                   5                   263B                0B (0%)
Local Volumes       58                  5                   784.7MB             428.7MB (54%)
Build Cache         0                   0                   0B                  0B
```

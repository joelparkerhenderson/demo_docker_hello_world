# Demo Docker for Mac

Demonstration of:

  * [Docker](https://docker.com) software containerization platform
  * [Docker Community Edition for Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac)


## Setup

Get Docker for Mac.

Option 1: Download a .dmg file:

   https://docs.docker.com/docker-for-mac/

Option 2: Use brew:

    $ brew cask install docker

Verify:

    $ docker --version
    Docker version 18.03.1-ce, build 9ee9f40

    $ docker-compose --version
    docker-compose version 1.21.1, build 5a3f1a3

    $ docker-machine --version
    docker-machine version 0.14.0, build 89b8332


## Launch

Launch Docker.app:

  * Sign in with your Docker id and password.

  * You should see a dialog "Docker is now up and running!"

  * You should see a Docker icon in your menu bar.


## Run

Run a "hello world" container:

    $ docker run hello-world
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    9bb5a5d4561a: Pull complete
    Digest: sha256:f5233545e43561214ca4891fd1157e1c3c563316ed8e237750d59bde73361e77
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
    3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/engine/userguide/

Experiment with various commands:

    $ docker images
    $ docker ps
    …

Stop the "hello world" container:

    $ docker stop hello-world


## Run a demo webserver

Run a webserver container, such as the NGINX webserver container:

    $ docker run -d -p 80:80 --name webserver nginx
    Unable to find image 'nginx:latest' locally
    latest: Pulling from library/nginx
    …

Browse http://localhost/ to bring up the home page.

  * You should see "Welcome to nginx!"

Experiment with various commands:

    $ docker images
    $ docker ps
    …

Stop the container:

    $ docker stop webserver


## Create a webserver using a Dockerfile

This section thanks to https://www.katacoda.com/courses/docker/create-nginx-static-web-server

Demonstration of:

  * A container based on Alpine Linux and Nginx webserver
  * How to create a Dockerfile

Create any directory:

    $ mkdir demo
    $ cd demo

Create a file named `Dockerfile` with this text:

    FROM nginx:alpine
    COPY . /usr/share/nginx/html

Build:

    $ docker build -t webserver-image:v1 .
    Sending build context to Docker daemon  2.048kB
    Step 1/2 : FROM nginx:alpine
    alpine: Pulling from library/nginx
    ff3a5c916c92: Pull complete
    d81b148fab7c: Pull complete
    f9fe12447daf: Pull complete
    ad017fd52da2: Pull complete
    Digest: sha256:4a85273d1e403fbf676960c0ad41b673c7b034204a48cb32779fbb2c18e3839d
    Status: Downloaded newer image for nginx:alpine
    ---> bc7fdec94612
    Step 2/2 : COPY . /usr/share/nginx/html
    ---> da3fb243e03a
    Successfully built da3fb243e03a
    Successfully tagged webserver-image:v1

Run:

    $ docker run -d -p 80:80 webserver-image:v1

Get the container's custom name:

    $ docker ps
    CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                NAMES
    7f5d8f957039        webserver-image:v1   "nginx -g 'daemon of…"   2 minutes ago       Up 2 minutes        0.0.0.0:80->80/tcp   blissful_bhaskara

Stop the container with its custom name

    $ docker stop blissful_bhaskara


### Troubleshoot: port is already allocated

Symptom:

    docker: Error response from daemon: driver failed programming 
    external connectivity on endpoint adoring_kapitsa 
    (3557175705a1c0c3974d31261528917946b698c9a7f9696a2a0bd82d03b87fa4): 
    Bind for 0.0.0.0:80 failed: port is already allocated.

Treatment:

  * Option 1: Find out what's using port 80 then stop it e.g. try `docker images`, try `docker stop hello-world`, try `docker stop webserver`.

  * Option 2: Use a different port e.g. `docker run -d -p 81:81 webserver-image:v1`

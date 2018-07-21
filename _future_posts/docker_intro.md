# Getting Started with Docker

## What is Docker?
Docker is a [container](https://en.wikipedia.org/wiki/Operating-system-level_virtualization).

While a Virtual Machine (VM) is an entire guest computer running on top of your
computer, or host machine, Docker is an isolated isolated portion of your computer. Docker shares the host kernel, operating system (OS), and sometimes its binaries and therefore more lightweight than a VM.

For example, if you are running a server with CentOS and have a Docker Container
based on Ubuntu, then the Docker Container will hae the parts that make Ubuntu
different from CentOS.

Here is an image provided by Docker that depicts the differences clearly:

![Docker slides image](/images/difference_docker_vm.png)

Originally from [these slides](https://www.slideshare.net/dotCloud/docker-intro-november).

## Why Docker
So why Docker? What problems does it solve for me? Docker can be useful in a few ways. 

  1. Docker containers are fast! You are able to create a new docker container, destroy one, pull from Docker Hub, and develop
    inside them with ease. Docker containers don't take many resources from your computer and are easy to manipulate. This allows
    a users to make reproducible development environments much faster than with a traditional Virtual Machine.
  2. Modularity. It is possible to break down your application functionality into individual containers. 
    It is easy to link containers together to create your application.
    One can easily scale and update components independently in the future. 
    For example, if you want to update your database, you can update to the newest version of postgres using the same base
    container as you used previously and then update your application without taking everything offline.
  3. Docker Hub! This is where there are over 100,000 containers already created. You can take these containers
    instead of starting from scratch. This saves you time and ensures that the enviornment is setup properly. 
    Sending and reciving containers removes the problem of "It worked on my computer but not yours". 

## Getting Docker

Docker has been moving fast since it was first released in 2013. Docker now offers an Enterprise Edition (EE) and a Comunity Edition (CE). Like most software companies, the EE version has more support but comes with an Annual subscription and the CE is for individual users and is free.

You can find the CE for your OS [here](https://www.docker.com/community-edition).
 If you are using Mac or Windows they will ask you to create a free account
before downloading. You will have to do this to use Docker Hub anyway, which
we will talk about later.
For more detailed instructions on how to download docker you can go [here](https://docs.docker.com/install).

## Your First Container
Here we will create your first container which may feel underwhelming.
It does not show off the full magnitude of why developers love docker,
but we must walk before we can run.
First make sure you have started Docker and allocated some of your systems
resources to Docker. The Good news is Docker will only take these resources
when it is activated.

Docker has images they call _base containers_.
These are containers that you can pull from [Docker Hub](https://hub.docker.com)
as you would with a package manager like _apt_ or _pip_.
These containers are normally the bare essentials for an OS like Ubuntu, or
have per-configured environments so you don't have to do the setup yourself.
These containers can have [Postgres](https://hub.docker.com/_/postgres/), [Spark](https://hub.docker.com/r/p7hb/docker-spark/), or [Ngix](https://hub.docker.com/_/nginx/) already configured. Explore all the base containers on Docker Hub
[Here](https://hub.docker.com/explore/).
You can pull down these containers and tailor them to your needs.

A simple container is called _Ubuntu_.
As you may have guessed, it creates a container that runs Ubuntu.
This will be a pretty minimal version of Ubuntu, so you will not have some
programs that are standard in a normal LTS version of Ubuntu like Vi.

Now it's finally time, let's make your first container by running the following
 in bash:

```sh
$ docker run ubuntu /bin/bash
```
You may say to yourself "Self, I didn't pull the Ubuntu image, how will this work?".

> If the image is not already downloaded in your system, it will download it first from the repository on Docker hub. Note the use of similar terminology to version control systems such as Git.

After you use the command above you can run ```docker ps``` to display all
active containers or ```docker ps -a``` to display all containers.
You should see:

```
CONTAINER      IMAGE         COMMAND    CREATED          STATUS         PORTS    NAMES
4c01db0b339c   ubuntu:18.04  bash       17 seconds ago   Up 16 seconds           nostalgic_bear
```
Let's breakdown this output quickly. 
  - **CONTAINER** is the ID for the specific container your just created.  
  - **IMAGE** is the base image the container was created from.  
  - **COMMAND** is the command used when you created the container.  
  - **CREATED** is how long ago the container was created.  
  - **STATUS** indicates wheither the container is still running or not.  
  - **PORTS** are the ports in use by the container.  
  - **NAMES** is the name of the specific container.  

You if you do not give your container a name, one will automatically be genereated. You can activate and deactivate 
containers by their name or ID.

Now that we have pulled an image and created an image, it time to actually do something with it! First we'll stop our container we
just created by running
```sh
$ docker stop nostalgic_bear
```
or whatever the name of your container is. Now we will create a new container using the ```-i``` and ```-t``` tags. Before we do it may be useful to find out what these are. We can use the ```--help``` tag to learn more.

```sh
$ docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
  -i, --interactive                    Keep STDIN open even if not attached
      --rm                             Automatically remove the container
  -t, --tty                            Allocate a pseudo-TTY
  -v, --volume list                    Bind mount a volume
  -p, --publish list                   Publish a container's port(s) to
                                       the host
```

The true list of options is quite extensive so I cut it down to the few commands that we will talk about here. It is fine if you are 
unfamiliar with [STDIN](https://en.wikipedia.org/wiki/Standard_streams) and [Pseudo-TTY](https://en.wikipedia.org/wiki/Pseudoterminal). 
Basically we are going to link our terminal to one in the container we create and it will run for as long as the container is active. 
Now if we run the following we can interact inside the container.

```sh
$ docker run -it ubuntu

root@f81b76b39a88:/#
```

As you can see we are now logged in as root in the container. As mentioned before, this is a lightweight version of Ubuntu and you will 
most likely need to install all the programs you are going to want or need. When you are all done you can simply enter ```exit``` to 
leave the container.

```sh
root@f81b76b39a88:/home# exit
exit

lwileczek:~/Documents$
```

This should stop the container as well. 

## Dockerfile
Most people don't want to create a docker container from scratch or pull a generic from Docker Hub 
each time they want to create a new container. They know what programs they want to use, perhaps they have some files they want 
transferd from their host machine into the container. The best way to do that is to create a [Dockerfile](https://docs.docker.com/engine/reference/builder/#usage) and use it to create your 
contianer.  Without getting to detailed right now, you can create a file and use ```docker build .``` and create a customized 
container. For example, let the following be your Dockerfile:

```
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

Each instruction creates one layer:
- FROM creates a layer from the ubuntu:15.04 Docker image.
- COPY adds files from your Docker client’s current directory.
- RUN builds your application with make.
- CMD specifies what command to run within the container.  

I took this example from Dockerfile best practices page linked at the bottom. 

## Connecting Your Container to Your Host Machine

[volumes](https://docs.docker.com/storage/volumes/) are a way to share information accross containers or between your container and host 
machine. You can use volumes to pass data files back and fourth or to allow you to use a text editor to change code you have in your 
container. There are [different types](https://success.docker.com/article/different-types-of-volumes) of volumes, but I'm going to 
show you how to connect a folder on your host to one in the container so you can edit some Perl code you may have.  Let's say you have 
the following folder:

```sh
~/Documents/my_folder$ ls
 my_code.pl  my_data.csv
```
You want to keep you container as small as possible so you don't want to install vim or you don't like it. To get around this you can
create a volume to connect this folder to one in the container so when you edit ```my_code.py``` the changes will be visible to both. 
We can create a volume by using the ```-v``` tag e.g.

```sh
$ docker run -v /path/on/host:/path/in/container ...
```
You must use a full path on both ends to connect the two folders properly. So if you are on windows you it would something like:

```sh
> docker run -v C:\Users\JohnSmith\Documents\my_folder:/home/my_folder -it ubuntu bash

root@accce7d7da8f:/# ls home/my_folder
 my_code.pl  my_data.csv
```

To show we can edit the file from our host machine see the edits in the container we must first check what the file looks like, then 
detach from our container, update the file then attach ourselves again. 

```sh
root@accce7d7da8f:/# cd home/my_folder
root@accce7d7da8f:/home/my_folder# cat my_code.pl
#!/usr/bin/perl
print "Hello, World!\n";
root@accce7d7da8f:/home/my_folder#
```
To detatch from a container we are currently in an interactive session with we can use ```ctrl+p+q```. You should see something similar
to: 

```sh
root@accce7d7da8f:/home/p# read escape sequence
```

This did not stop the container. We can verify this by running ```$ docker ps```.

... to be continued

### More Resources:
  * [Getting Started](https://docs.docker.com/get-started/) The introduction documentation on Docker's website. This is kept current and may be your best resource.
  * [Servers for Hackers](https://serversforhackers.com/c/getting-started-with-docker) This is the blog I took most of my information from originally. It was written back in 2014 and is now a little out of date.
  * [Learn Docker in 12 Minutes](https://www.youtube.com/watch?v=YFl2mCHdv24r) A YouTube video that dives in and explains Docker pretty well.   
  * [Docker Curriculum](http://docker-curriculum.com/) Written back in 2015, but shows how to get get started with AWS and web applications. 
  * [Dockerfile Best-Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)


Docker information

    docker info
    docker version
    
Location where docker images live

    /var/lib/docker/image
    /var/lib/docker/image/devicemapper/imagedb/content/sha256

Location where docker container lives

    /var/lib/docker/containers

Pulling ubuntu xenial image

    docker pull ubuntu:xenial
    docker pull nginx:latest

    # -i interactive mode
    # -t attached to my terminal (tty)
    docker run -i -t ubuntu:xenial /bin/bash

Restart docker container

    docker restart awesome_sinoussi

Inspect an image

    docker inspect ubuntu:xenial


Attach to a container

    docker attach awesome_sinoussi

Run a container as a deamon

    # -i interactive mode
    # -t attached to my terminal (tty)
    # -d demonized
    docker run -itd ubuntu:xenial /bin/bash

Inspect a container 

    docker inspect reverent_thompson

Start a container

    docker start focused_kilby

Stop a container

    docker stop reverent_thompson

Search docker images

    docker search ubuntu
    docker search training/sinatra

Remove docker image

    docker rmi training/sinatra
    
Build a docker image

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com
    RUN useradd -m -s /bin/bash user
    USER user

    docker build -t centos7/nonroot:v1 .
    docker run -it centos7/noroot:v1 /bin/bash

Connect to a container as root

    docker exec -u 0 -it focused_kilby /bin/bash

Running nginx in docker

    docker pull nginx:latest
    docker run -d nginx:latest

    # This way of running redirect the local port 80 to the container port 80
    docker run -d -p 80:80 nginx:latest
    elinks http://172.17.0.3
    
    # host -> 8080, container -> 80
    docker run -d -p 8080:80 nginx:latest
    elinks http://127.0.0.1:8080
    elinks http://172.17.0.3

Checking container logs

    docker logs sharp_elion

Delete all containers

    docker ps -a | cut -d' ' -f1 | tail -n+2 | xargs docker rm

Commit a container

    docker commit -m "Already install SSH and created test user" -a "pavelsimo" clever_panini pavelsimo/ubuntusshd:v1
    docker run -it pavelsimo/ubuntusshd:v1 /bin/bash

Simple Dockerfile with SSH

    # This is a custom ubuntu image with SSH already installed

    FROM ubuntu:xenial
    MAINTAINER pavelsimo <pavel.simo@gmail.com>
    RUN apt-get update
    RUN apt-get install telnet openssh-server

Building the image

    # if the Dockerfile is in the same directory
    docker build -t="pavelsimo/ubuntusshonly:v2" .

    # if the Dockerfile exist in another directory
    docker build -t="pavelsimo/ubuntusshonly:v2" < /path/to/Dockerfile
 
    # another way of building an image
    docker build -t centos7/nonroot:v1 .

    # running the image
    docker run -it pavelsimo/ubuntusshonly:v2 /bin/bash

See a container logs

    docker logs tender_kalam

Execute container command

    docker exec tender_kalam /bin/cat /etc/profile

Run a command as a daemon:

    docker run -d ubuntu:xenial /bin/bash -c "while true; do echo HELLO; sleep 1; done"

Expose a container port

    # 8000 - HOST PORT
    # 80   - CONTAINER PORT
    docker run -d -p 8000:80 nginx:latest

    # exposing more than one port
    docker run -itd -p 8080:80 -p 8081:443 nginx:latest

    # exposing only in the localhost
    docker run -itd -p 127.0.0.1:8000:80 nginx:latest

Remove a Docker Image

    docker rmi e0870be525d9

Connect to container as root

    docker exec -u 0 -it sharp_mayer /bin/bash

Another container example

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com

    RUN useradd -ms /bin/bash user
    RUN echo "EXPORT 192.168.0.0/24" >> /etc/exports.list

    USER user

Creating a Java Container

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com

    RUN useradd -ms /bin/bash user
    RUN echo "EXPORT 192.168.0.0/24" >> /etc/exports.list

    RUN yum update -y
    RUN yum install -y net-tools wget
    RUN cd ~ && wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jre-8u60-linux-x64.rpm"

    RUN yum localinstall -y ~/jre-8u60-linux-x64.rpm

    USER user

    RUN cd ~ && echo "export JAVA_HOME=/usr/java/jdk1.8.0/jre" >> /home/user/.bashrc
    ENV JAVA_BIN /usr/java/jdk1.8.0/jre/bin

Using the Dockerfile ENV directive to create an env variable for all users

    ENV JAVA_BIN /usr/java/jdk1.8.0/jre/bin

The Dockerfile CMD directive it's used to run commands after the container is created, it differs from the RUN directive which create a file system layer.

    CMD "echo" "Hello World!"

The Dockerfile ENTRYPOINT directive, enforce the default behaviour each time our container is created, it differs from CMD which can be alter by the default program that we run with the container.

    ENTRYPOINT echo "This message will always appear"

Run a container with a name

    docker run -d --name apacheweb1 centos7/apache:v1    
    docker run -d --name apacheweb3 -p 8080:80 centos7/apache:v1

Exposing ports in a Dockerfile

    FROM centos:latest
    MAINTAINER pavel.simo@gmail.com

    RUN yum update -y
    RUN yum install -y httpd net-tools

    RUN echo "This is a custom index file build during the image creation" > /var/www/html/index.html

    EXPOSE 80

    ENTRYPOINT apachectl "-DFOREGROUND"

The docker run -P option automatically map all the ports exposed by the container

    docker run -d --name apacheweb4 -P centos7/apache:v1 

Create a container with host volume

    docker run -it --name voltest1 -v /data centos:latest /bin/bash

Create a container with host volume in a specific directory

    docker run -it --name voltest2 -v /root/Builds/MyHostDir:/data centos:latest /bin/bash

If you run into permission denied issues could be related with selinux two solutions:

    # http://stackoverflow.com/questions/24288616/permission-denied-on-accessing-host-directory-in-docker
    su -c "setenforce 0"

    # http://www.projectatomic.io/blog/2015/06/using-volumes-with-docker-can-cause-problems-with-selinux/
    # notice the use of :Z at the end, this will let selinux knows about this sandbox volume
    docker run -it --name voltest6 -v /root/Builds/MyHostDir:/data2:Z centos:latest /bin/bash
    
List all the docker networks associated with the current host

    docker network ls

    # don't truncate the network id
    docker network ls --no-trunc

Get information about the bridge network

    docker network inspect bridge

Get the man pages for a composite docker command

    # man page for the command docker network create
    man docker-network-create

    # man page for the command docker run
    man docker-run

Create a docker network

    docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 mybridge01
    docker network create --subnet 10.1.0.0/16 --gateway 10.1.0.1 --ip-range=10.1.4.0/24 --driver=bridge --label=host4network bridge04

Delete a docker network

    docker network rm mybridge01

Run a container within a given network

    docker run -it --name nettest1 --net bridge04 centos:latest /bin/bash

    # using a specific ip address, notice this just work for custom networks, will not work for docker0 bridge
    docker run -it --name nettest2 --net bridge04 --ip 10.1.4.100 centos:latest /bin/bash

Display the running process in a container

    docker top tender_kalam

Running a container bash (the container will not close on exit)

    docker exec -i -t tender_kalam /bin/bash

Display a live stream of a container(s) resource usage statistics

    docker stats tender_kalam nettest2

Counting the previous run containers

    docker ps -a -q | wc -l

Removing a running container

    docker rm -f tender_kalam

Delete all stopped containers

    docker rm `docker ps -a -q`

You can manually delete a container by deleting it from the docker containers directory

    systemctl stop docker
    rm -rf /var/lib/docker/containers/04493c3d458a4b35b0df98969a28ef467aaac3556f94a411ca59890f23a75d0c

Naming a container

    docker run -itd -P --name mynginx_container nginx:latest /bin/bash

Renaming a container

    docker rename mynginx_container mycrazycontainer

Wait for Docker Events

    docker events

Show all the events that have happen in the last hour

    docker events --since '1h'

Kill a container (default signal SIGKILL)

    docker kill clever_panini

Listen to attach events

    docker events --filter event=attach

Listen to multiple events

    docker events --filter event=attach --filter event=die --filter event=stop

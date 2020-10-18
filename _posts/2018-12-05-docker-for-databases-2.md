---
layout: post
title:  "Docker for Databases (Part 2)"
description: Actual implementation of running MySQL inside a Docker container.
author: Ajinkya
categories: [ docker, tutorial ]
image: assets/images/2018-12-05-docker-for-databases-2/docker.png
---
In Part 1 of this series, you were introduced with the Databases and Volumes in Docker, their significance in Data Persistence and 2 approaches of using them in Container. 

In this part, I will introduce you to an actual implementation of running MySQL inside a Docker container.

## The "Traditional" way VS The "Containerized" way

![placeholder]({{ site.baseurl }}/assets/images/2018-12-05-docker-for-databases-2/docker9.jpg)

The traditional way to run a MySQL database is to install the MySQL packages on a host (bare-metal, virtual machine, cloud instance), and applications would just have to connect to the listening port.

Most of the management tasks, for example, configuration tuning, backup, restore, database upgrade, performance tweaking, troubleshooting and so on have to be executed on the database host itself. You would expect to have several ports accessible for connection, for example, port <code>TCP 22</code> for <code>SSH</code>, <code>TCP 3306</code> for <code>MySQL</code> or <code>UDP 514</code> for <code>syslog</code>.

In a container, think of MySQL as one single unit that only serves MySQL related stuff on port 3306. Most of the operation should be performed under this single channel. Docker works great in packaging your application/software into one single unit, which you can then deploy anywhere as long as Docker engine is installed. It expects the package, or image to be run as a single process per container.

> With Docker, the flow would be: you (or someone) build a MySQL image using a specific version and vendor, package the image and distribute to anybody who wants to quickly fire a MySQL instance.

## Tutorial

To start with, find a container image that you want from the Docker registry. It can be a public registry like <code>Docker Hub</code> or a private registry, where you host the containers’ image on-premises, within your own network. If you can’t find the image that fits you, you can build your own.

There are many MySQL container images available in the Docker Hub registry. The following screenshot shows some examples:

![placeholder]({{ site.baseurl }}/assets/images/2018-12-05-docker-for-databases-2/docker4.png)

It is always advised to use an <code>Official</code> image over <code>Customized</code> ones.

To start a MySQL container instance,

```shell
    docker run -d --name test-mysql \
    > -e MYSQL_ROOT_PASSWORD=yourpass \
    > -p 6603:3306 mysql
```

You will get an output of the container ID, indicating the container is successfully running in the background. Let’s verify the status of the container:

```shell
    docker ps
```

It will display the details of the running container along with the port mappings.

```shell
CONTAINER ID      IMAGE           COMMAND             CREATED  
    STATUS                      PORTS                 NAMES
83285aa548a    mysql:latest  docker-entrypoint.s  4 minutes ago
Up 4 minutes           0.0.0.0:6603-> 3306/tcp     test-mysql
```

![placeholder]({{ site.baseurl }}/assets/images/2018-12-05-docker-for-databases-2/docker5.png)


Now, if you want your MySQL Database to be linked with another service running inside another container, all you have to do is to ‘Link’ that container with your MySQL container.

Let’s say, you want to link your MySQL DB to a Wordpress container.
You can do that by spinning up a Wordpress Image while linking it to MySQL container that is already running.

```shell
    docker run --detach --name test-wordpress \
    > --link test-mysql:mysql wordpress
```

![placeholder]({{ site.baseurl }}/assets/images/2018-12-05-docker-for-databases-2/docker6.png)


## Using Volumes in MySQL Container

The best way to persist data of MySQL is to create a data directory on the host system (outside the container) and mount this to a directory visible from inside the container. This places the database files in a known location on the host system and makes it easy for tools and applications on the host system to access the files.

A user needs to make sure that the directory exists, and that e.g. directory permissions and other security mechanisms on the host system are correctly set up.

Now, Create a data directory on a suitable volume on your host system, e.g. /storage/docker/mysql-datadir:

```shell
    mkdir -p /storage/docker/mysql-datadir
```

Now, start your MySQL container like this:

```shell
    docker run --detach --name=test-mysql \
    > --env="MYSQL_ROOT_PASSWORD=mypassword" \
    > --publish 6603:3306 \
    > --volume=/root/docker/test-mysql/conf.d:/etc/mysql/conf.d \
    > --volume=/storage/docker/mysql-datadir:/var/lib/mysql \
    > mysql
```

Here, we are binding 2 directories from the Host system with the ones inside the container.

```shell
    --volume=/root/docker/test-mysql/conf.d:/etc/mysql/conf.d
    --volume=/storage/docker/mysql-datadir:/var/lib/mysql
```

![placeholder]({{ site.baseurl }}/assets/images/2018-12-05-docker-for-databases-2/docker8.png)

Because of this binding, restarting or removing the container does not remove the MySQL data directory. When you restart a MySQL container by using “stop” and “start” command, it would now be similar to restarting the MySQL service in a standard installation.

If you remove the MySQL container, the data in the mounted volumes will still be intact and you can run a new instance, mounting the same volume as data directory.

<b>That's it!</b>

## Conclusion

This completes my 2 part <code>Docker for Databases</code> series. You can check out Part 1 if you missed it somehow.
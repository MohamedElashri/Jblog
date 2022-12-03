---
layout: post
title: " remove docker from ubuntu "
description: "How to uninstall docker on ubuntu."
keywords: "docker, ubuntu, debian, apt-get"
comments: true
---

-----------------------

The docker command provides several options for managing Docker containers and images. One of these options is the `system prune` command, which can be used to remove unused data from the Docker system. This command can be used to "nuke" everything Docker, by removing all unused containers, images, networks, and volumes. Later you can safely remove docker from the system.

To nuke everything Docker, you can use the following script:

``` bash
# Stop and remove all running containers
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)

# Remove all unused images
docker rmi $(docker images -q)

# Remove all unused networks
docker network prune -f

# Remove all unused volumes
docker volume prune -f
```

This code uses stops and removes all running containers, removes all unused images, removes all unused networks, and removes all unused volumes. The `-f` option is used with the `network prune` and `volume prune` commands to force the removal of the unused data.

Note that using the `system prune` command or the script above will remove all unused data from the Docker system. This can free up a significant amount of disk space, but it can also remove data that you may want to keep. Therefore, it is recommended to use this command with caution and only when you are sure that you want to remove all unused data from the Docker system.

Now to remove docker from ubuntu, you can use the following commands:

``` bash
sudo apt-get purge -y docker-engine docker docker.io docker-ce;
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce;
sudo rm -rf /var/lib/docker sudo rm /etc/apparmor.d/docker sudo groupdel docker sudo rm -rf /var/run/docker.sock;
```

All of these can be written in a simple nice bash script for automation.

``` bash
#!/bin/bash

## Stop and remove all containers
echo "Removing containers :"
if [ -n "$(docker container ls -aq)" ]; then
  docker container stop $(docker container ls -aq);
  docker container rm $(docker container ls -aq);
fi;

## Remove all images
echo "Removing images :"
if [ -n "$(docker images -aq)" ]; then
  docker rmi -f $(docker images -aq);
fi;

## Remove all volumes
echo "Removing volumes :"
if [ -n "$(docker volume ls -q)" ]; then
  docker volume rm $(docker volume ls -q);
fi;

## Remove all networks
echo "Removing networks :"
# Skip default networks : bridge, host, none
if [ -n "$(docker network ls | awk '{print $1" "$2}' | grep -v 'ID\|bridge\|host\|none' | awk '{print $1}')" ]; then
  docker network rm $(docker network ls | awk '{print $1" "$2}' | grep -v 'ID\|bridge\|host\|none' | awk '{print $1}');
fi;


## Remove docker engine and clean
echo "Uninstall and clean docker :"

if [ -n "$(dpkg -l | grep -i docker )" ]; then
  sudo apt-get purge -y docker-engine docker docker.io docker-ce;
  sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce;
  sudo rm -rf /var/lib/docker sudo rm /etc/apparmor.d/docker sudo groupdel docker sudo rm -rf /var/run/docker.sock;
fi;

echo "Done :"
```


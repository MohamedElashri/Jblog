---
published: true
layout: post
title: "Self-hosted firefox sync server"
description: "How to run firefox sync server on your own server"
comments: true
keywords: "firefox, sync, server, self-hosted, firefox sync server"
---

Firefox Sync is a powerful feature that allows you to keep your bookmarks, history, and other data in sync across all of your devices. It’s a great way to ensure that your favorite websites and browsing habits are always with you, no matter where you are.

If you’re a privacy-conscious user like me, you may be hesitant to rely on Mozilla’s servers to store your data. Fortunately, it’s possible to run your own Firefox Sync server, giving you complete control over where your data is stored and how it is accessed. 

In this blog post, I will walk you through the steps for setting up your own Firefox Sync server. This installation method is going to be docker based, so you will need to have docker installed on your server. If you don’t have docker installed, you can follow the instructions here to install it. You can find instructions for doing this on the [Install Docker-Engine](https://docs.docker.com/engine/install/ubuntu/). Also you will need to install docker-compose, this is also available on the [Install docker-compose](https://docs.docker.com/compose/install/).

Alternatively, you can use the following shell script to install docker and docker-compose on your server:

``` bash
#!/bin/bash

apt-get update -y
apt-get install -y apt-transport-https ca-certificates curl software-properties-common

sudo mkdir -p /etc/apt/keyrings
 

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update -y

apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```

Next, create a file called docker-compose.yml in a new directory on your server. This file will contain the configuration for your Firefox Sync server. In this file you will need to add the following configuration:

``` yaml
version: "3.2"

services:
  firefox-syncserver:
    image: crazymax/firefox-syncserver:latest
    container_name: firefox
    ports:
      - target: 5000
        published: 5000
        protocol: tcp
    volumes:
      - "/home/<user>/firefox-sync/data:/data"
    env_file:
      - "/home/<user>/firefox-sync/firefox.env"
    restart: always

volumes:
  firefox-syncserver:
```

This configuration will create a docker container that will run the Firefox Sync server. It will also create a volume that will store the data for the server. You will need to replace the <user> with your username on the server. You can also change the port that the server is running on, if you want to. One important thing is that you will need to pass a .env file to the container. This file will contain the configuration for the server. You can find an example of this file in the following code 

``` bash
TZ=America/NewYork
PUID=1000
PGID=1000
FF_SYNCSERVER_PUBLIC_URL=http://<server>.<domain>
FF_SYNCSERVER_SECRET=736hdojh929jdmo
FF_SYNCSERVER_ALLOW_NEW_USERS=true

FF_SYNCSERVER_FORCE_WSGI_ENVIRON=false
```

The explnation for each of these variables is as follows:

    - `TZ`: This variable specifies the time zone for the application or service. In this case, the value is set to America/NewYork, indicating that the time zone is Eastern Standard Time.

    - `PUID` and `PGID`: These variables specify the user and group IDs for the application or service. In this case, the values are set to 1000, indicating that the application or service will run as the user and group with IDs 1000.

    - `FF_SYNCSERVER_PUBLIC_URL`: This variable specifies the public URL for the Firefox Sync server. In this case, the value is set to http://<server>.<domain>, indicating that the server is accessible at the specified domain name.

    - `FF_SYNCSERVER_SECRET`: This variable specifies a secret value that is used to authenticate requests to the Firefox Sync server. In this case, the value is a long, random string of characters.

    - `FF_SYNCSERVER_ALLOW_NEW_USERS`: This variable specifies whether new users are allowed to register for the Firefox Sync server. In this case, the value is set to true, indicating that new users are allowed to register.

    - `FF_SYNCSERVER_FORCE_WSGI_ENVIRON`: This variable specifies whether the WSGI environment should be forced when running the Firefox Sync server. In this case, the value is set to false, indicating that the WSGI environment will not be forced.


Once you’ve created your docker-compose.yml file and added your `firefox.env` file, you can start your Firefox Sync container by running the following command:

``` bash
docker-compose up -d
```

This will pull the crazymax/firefox-syncserver image from the Docker Hub, create a new container using the configuration in your docker-compose.yml file, and start the container in the background. After your container is up and running, you can configure your Firefox browser to use your new Sync server. This can be done by going to the Sync settings in Firefox and entering the URL for your server. To do this you will need to modify `identity.sync.tokenserver.uri` in `about:config` . First you will need to go to `about:config` and then search for `identity.sync.tokenserver.uri` and change the value to the URL of your server. You can find the default value for this variable to be:

``` 
https://token.services.mozilla.com/1.0/sync/1.5
``` 
You will need to change this value to the URL of your server. Adjust the URL to match the domain name of your server. For example, if your server is running at `https://<server>.<domain>`, you will need to change the value to:

```
https://<server>.<domain>/1.0/sync/1.5
```

Once you’ve done that, you’ll be able to start syncing your data to your own server. You can then use the same account on all of your other devices to keep your data in sync across all of them.
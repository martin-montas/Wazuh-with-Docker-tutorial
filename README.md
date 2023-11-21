# Wazuh-with-Docker-tutorial
---

This is a simple tutorial that anybody can follow to get started with Wazuh IDS on  Debian-based Linux systems. Before we start,
It's recommended to set up the Wazuh Manager on a different server as it can create
a great deal of log files.

### Setting up Docker and Git:
---

step 1. First of all, you should make sure that `Curl`, `Docker` and `Git` are installed:

to do this, run:
```sh 
$ which curl                                                                                                     
```

```sh
$ which docker                                                                                                                 
```

```sh
$ which git                                                                                                                 
```


if you don't see the output, move to step 2, and if you do skip it.


step 2. you should have `Docker` and `Curl` installed on the server. So open a terminal and type:
```sh 
$ sudo apt-get update                                                                                                                                                                             
```
then run:
```sh
$ sudo apt-get install curl                                                                                                          
```
and:
```sh
$ sudo apt-get install  git                                                                                                          
```

when everything is updated, run the following command for installing `Docker`:
```sh
$ curl -fsSL https://get.docker.com/ | sh                                                                                              
```

now you should have `Docker` installed. To test it run this command:
```sh
$ docker run hello-world                                                                                                                         
```

then you should restart `Docker`:
Run:
```sh
$ sudo service docker restart                                                                                                   
```
everything should be fine now.

### Installing Wazuh:
---

step 1. To clone the  `Docker` image for `Wazuh`, you will need to clone
this GitHub repository on the main server:

```sh
$ git clone https://github.com/wazuh/wazuh-docker.git -b v4.5.4                                                                  
```
step 2. you should then go to the wazuh-docker directory recently installed:
```sh
$ cd wazuh-docker
```
We are keeping things simple so in this tutorial, we'll be installing wazuh
as a sigle_node. This means that we will install a single manager, indexer, and a dashboard.

step 3. Go inside the single_node directory:
```sh
$ cd wazuh-docker/single-node
```
step 4. Now comes an optional step based on your configuration. If your system uses a proxy you 
should add the following line to the `generate-indexer-certs.yml`  file in the directory. else
skip to step 5.

```yaml
environment:
  - HTTP_PROXY=YOUR_PROXY_ADDRESS_OR_DNS
```
this is how it should look:
```yaml
# Wazuh App Copyright (C) 2021 Wazuh Inc. (License GPLv2)
version: '3'

services:
  generator:
    image: wazuh/wazuh-certs-generator:0.0.1
    hostname: wazuh-certs-generator
    volumes:
      - ./config/wazuh_indexer_ssl_certs/:/certificates/
      - ./config/certs.yml:/config/certs.yml
    environment:
      - HTTP_PROXY=YOUR_PROXY_ADDRESS_OR_DNS
```
step 5. Now start the wazuh docker image you should run the following
command on the current directory:

```sh
$ docker-compose up -d
```
This command will take a while so be patient.

step 6. To be sure that `Wazuh` is installed run the following command:
```sh
$ docker ps
```
you should be able to see  each container and their correspondent name and port

### Setting up the firewall:

in case you can't connect the manager to one of the agents, you must make the firewall
allow the corresponding ports on your `Wazuh` agent node and the manager itself. To make things
simple, install `ufw` firewall with this command:

```sh 
$ sudo apt install ufw
```
to enable, run:
```sh
$ sudo ufw enable
```

you should then allow the following ports on the manager server.
Run the following commands:

```sh
$ sudo ufw allow 1514/tcp
$ sudo ufw allow 1514/udp
$ sudo ufw allow 1515/tcp
$ sudo ufw allow 55000/tcp
```
### Setting up agents:

you should now be able to go into the `Wazuh` dashboard 
with the corresponding manager password (you can find it in the docker `README.md` file for wazuh-docker).

Once you are on this stage it will be easy to set up agents on the machines that you want using the wazuh dashboard.
and that's it. I hope you could completed. If you have more questions that I left unanswered you see the wazuh documentation
at https://documentation.wazuh.com/current/index.html

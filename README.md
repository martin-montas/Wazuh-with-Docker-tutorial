# Wazuh-with-Docker-tutorial

This is a simple tutorial that anybody can follow to get started with Wazuh IDS on Linux.
It's recommended to set up the Wazuh Manager on a different server as it can create
a great deal of log files.

### Setting up Docker:

step 1. First of all you should make sure that `Curl` `Docker` is installed:

to do this, run:
```$ which curl```

```$ which docker```
if you don't see ouput, move to step 2 and if you do skip.


step 2. you shoud have Docker and curl installed on the server. So open a terminal and type:
```$ sudo apt-get update```
and:
```$ sudo apt-get install curl```

when everything is updated, run the following command for installing `Docker`:
```$ curl -fsSL https://get.docker.com/ | sh```

now you should have `Docker` installed. To test it run this command:
```$ docker run hello-world```

then you should restart `Docker`
```$ sudo service docker restart```
everything should be fine now.

### Installing Wazuh:

step 1. to install wazuh thru the a `Docker` image, you will need to clone
this GitHub repository on your system:

```git clone https://github.com/wazuh/wazuh-docker.git -b v4.5.4```





# Introduction
Docker image with Owasp Zed Attack Proxy preinstalled.
For GUI access this image supports VNC access.

# Details
## Install instructions:
`docker pull owasp/zap2docker-stable`

Or if you want to try the latest weekly release:

`docker pull owasp/zap2docker-weekly`

## Usage instructions:
### ZAPR:
Zapr is ruby script for ZAP which allows commandline active scanning for desired target:

`docker run -u zap -i owasp/zap2docker-stable zapr --debug --summary http://target`

### ZAP headless:
You can also start the ZAP in headless mode with following command:

`docker run -u zap -p 8080:8080 -i owasp/zap2docker-stable zap.sh -daemon -port 8080`

### ZAP GUI VNC:
If you wish to use the  zap gui you can start the embedded vnc-server:

`docker run -u zap -p 5900:5900 -p 8080:8080 -i owasp/zap2docker-stable x11vnc --forever --usepw --create`

This will first ask you to set vnc server password, once done it will startup the VNC session. Which you can connect with your vnc client (eg. in example its tcp port 5900). Note, you might need to set zap listen address to your dockers hostname instead of localhost. You can set this with -h parameter: docker run -h zap -i owasp/zap2docker-stable zap.sh command

### Accessing the API from outside of the Docker container:

Docker appears to assign 'random' IP addresses, so an approach that appears to work is:

Run ZAP as a daemon listening on "0.0.0.0":

`docker run -p 8090:8090 -i owasp/zap2docker-stable zap.sh -daemon -port 8090 -host 0.0.0.0`

Find out the container id:
`docker ps`

Find out which address has been assigned to it:
`docker inspect <<CONTAINER ID>> | grep IPAddress`

You should be then able to point your browser at the specified host/port and access the ZAP API, eg http://172.17.0.8:8090/
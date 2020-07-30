# Installing docker, and docker-compose

## Installation
### Linux
Update apt cache, remove old versions of docker, install and enable docker to start on boot
```
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```
Add your user to the `docker` group:
```
sudo gpasswd -a alex docker
```
Install docker-compose:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

### Mac
Follow this guide: https://docs.docker.com/docker-for-mac/install/

Docker Desktop for Mac and Docker Toolbox already include Compose along with other Docker apps, so Mac users do not need to install Compose separately.

### Windows
Follow this guide: https://docs.docker.com/docker-for-windows/install/

Docker Desktop for Windows and Docker Toolbox already include Compose along with other Docker apps, so most Windows users do not need to install Compose separately.

## Running docker-compose
If you run `docker-compose up -d` in the root of this project, it should build the images as necessary, and start the containers.

You can see them running via `docker ps`

You can stop and remove the containers by running `docker-compose down` (!) This will REMOVE containers, so may delete databases.

You can stop and NOT remove containers by running `docker-compose stop`.
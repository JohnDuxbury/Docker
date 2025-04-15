# Docker
Docker Learning

Archived Project
[![Build Status](https://drone.io/github.com/JohnDuxbury/Docker/status.png)](https://drone.io/github.com/JohnDuxbury/Docker/latest)

2025 Projects:
***Install Docker on the RPi 5***
https://docs.docker.com/engine/install/debian/

sudo apt update && sudo apt upgrade -y
reboot
mkdir -p /opt/stacks/docker
cd /opt/stacks/docker
curl -fsSL https://get.docker.com -o install-docker.sh
cat install-docker.sh
sh install-docker.sh --dry-run
sudo sh install-docker.sh
logout
-Check groups - user neesd to be a member of 'docker'
**Install Portainer**
docker pull portainer/portainer-ce:latest
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
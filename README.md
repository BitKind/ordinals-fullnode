# ordinals-fullnode
Ansible Playbook for Ordinals Full Node Build - Linux and Ubuntu

# Ubuntu Setup from cmd line
```
docker pull ubuntu:latest
docker run -idt --name ord ubuntu:latest
docker exec ord apt update -y && apt upgrade
docker exec ord apt install -y sudo
docker exec ord apt install -y python3
docker exec ord apt install -y python3-pip
docker exec ord python3 -m pip install ansible
docker cp ubuntu.yaml ord:/tmp/
docker exec ord ansible-playbook /tmp/ubuntu.yaml
```

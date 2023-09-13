# ordinals-fullnode
Ansible Playbook for Ordinals Full Node Build - Linux and Ubuntu
The images produced do not include final /var/bitcoin/data directory setup

The playboook installs Bitcoin, Rust, and compiles https://github.com/gmart7t2/ord.git https://github.com/casey/ord.git

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
# Oracle Linux 8 Setup from cmd line
```
docker pull oraclelinux:8
docker run -itd --name ord oraclelinux:8
docker exec ord dnf install -y python3.9
docker exec ord ansible-playbook /tmp/oel.yaml
docker exec ord python3 -m pip install ansible
```

# Committing image and Running with attached volume

```
docker ps -a | grep ord | cut -d" " -f1   #windows with cygwin or ubuntu
docker commit <containerid> ordinals:latest
# run the ordinals image and attach to a volume, naming the volume /bitcoin
docker run -idt --volume //<drive>/ordinals:/ordinals --volume //d/bitcoin:/bitcoin --name ord ordinals:latest
```
<img width="856" alt="image" src="https://github.com/BitKind/ordinals-fullnode/assets/120213/5e5a0381-1c9f-4769-be89-4abb2eba444a">

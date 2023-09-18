# ordinals-fullnode
Ansible Playbook for Ordinals Full Node Build - Linux and Ubuntu
The images produced do not include final /var/bitcoin/data directory setup

The playboook installs Bitcoin, Rust, and compiles https://github.com/gmart7t2/ord.git https://github.com/casey/ord.git


Window has RPC URL issues at volume, this appears disk/rpc call timeout related, as example, Disk usage may go to 100% during index commit at 5000, take a very long time depending on disk speed and then blow out, requiring index rebuild or index recovery if using latest ord versions.

Recommendation is to either run from Windows Ubuntu, or as done here, using Ord compiled in either Casey or Greg version within the instance.  The Ansible below compiles Ord in both versions, renaming Gregs versions to ordg.

For a full list of the lastet ordg options, see https://github.com/gmart7t2/ord/blob/master/src/options.rs


Example Archictures
```
A) Full Windows Native Full Bitcoin Node - Docker Localhost --rpc-url
Ubuntu Docker Instance - Native Ordg installed and complied

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
# run the ordinals image with localhost access and attach to a volume, naming the volume /bitcoin
docker run -idt --network="host" --volume //z/ordinals:/ordinals --volume //z/bitcoin:/bitcoin --name ord ordinals:latest
docker attach ord
sudo su - token
# make sure Bitcoin full node is running
# this example uses Bitcoin RPC installed natively on the host machine, not the installed docker Bitcoin
ord --bitcoin-data-dir /bitcoin/data --data-dir /bitcoin/data --index /ordinals/08-index-sats.redb --index-sats --rpc-url host.docker.internal:8332/wallet/ord wallet inscriptions
```
<img width="856" alt="image" src="https://github.com/BitKind/ordinals-fullnode/assets/120213/5e5a0381-1c9f-4769-be89-4abb2eba444a">

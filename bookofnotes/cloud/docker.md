# Docker

ports:

    2376/tcp - docker config port?

    2377/tcp - cluster management comms
    7946/{tcp,udp} - comm between nodes
    4789/udp overlay network

if encripted network uses ip proto 50 (ESP)

user: docker/tcuser

docker image pull swarm (before creation)

shell has some environment variables regarding to which docker-machine to connect
env | grep DOCKER

set environment to work with machine
eval `docker-machine env machine_name`

if weird errors happens about connecting, check environment variables, maybe not set correctly

## centos

```sh

# bad, dont do: yum install docker

# add mirror repo
wget https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
#replace download.docker.com with mirrors.tuna.tsinghua.edu.cn/docker-ce
yum install docker-ce

#in centos 7.2
yum install docker-ce-17.06.1.ce

# in the centos i tested centos would complay about overlay2 and xfs needing to reformat
edit daemon.json, add:
"storage-driver": "overlay"




# For china, edit /etc/docker/daemon.json, add:
# "registry-mirrors": ["https://registry.docker-cn.com"]
# this also works on docker tools, you must docker-machine ssh and do there

systemctl start docker


docker image pull mysql:5.7





```

## windows

win10 pro: docker-for-windows (uses hyperv)
other windows: docker toolbox (uses virtualbox)

After install run Docker quickstart terminal it does some initial configuration
and downloads some files

If want to save machines on a custom folder (not default ~/.docker/machine/...)
then set environment variable MACHINE_STORAGE_PATH and it is what it will use.

```sh

docker --version
docker info

docker image  ...

docker container ...

docker run ...

# remove container after finish
docker run ... --rm

# run interactive session
docker run -it <image> <command>

# detach from interactive session: Press ctrl+p ctrl+q
# attach to running session: press enter twice after this command
docker attach <session>

# list sessions
docker ps

# download image locally. Note that this is automatic when you "run".
# docker image pull == docker pull
docker image pull image/name
docker pull mysql:5.6

# get things from china registry
docker pull registry.docker-cn.com/library/mysql:5.6

docker run --name testmysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:5.6
docker run -it --link some-mysql:mysql --rm mysql:5.6 sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
docker run -it --rm mysql mysql -hsome.mysql.host -usome-mysql-user -p

# run with -p port:port to share a port, -P is not sharing it

# shell has some environment variables regarding to which docker-machine to connect
env | grep DOCKER

# set environment to work with machine
eval `docker-machine env machine_name`


# add remote host to docker-machine: 1- create keys if don't have
ssh-keygen -t rsa

# add remote host to docker-machine: 2- add it
docker-machine create -d generic
    --generic-ip-address <ip>       # MUST: the accesible ip of the machine
    --generic-engine-port <port>    # default: 2376
    --generic-ssh-key <path/to/key> # default to user default
    --generic-ssh-port <port>       # default 22
    --generic-ssh-user <user>       # default: 'root'


# to check: when doing reprovisioning, the hostname might be rewritten
# checked: regenerate-certs does change the hostname



```

## swarm

```sh

#docker-machine ssh name
#docker swarm init --advertise-addr 

# when create machine, add the --swarm parameter, otherwise somethings might fail
docker-machine create ... --swarm

# NOTE the ports must be forwarded and not firewalled

# init the master
# if it is a virtual machine, the advertise ip:port should be public and forwarded
# and listen-addr should be the one of the the virtual network card which receives
# the forward
docker swarm init --advertise-addr <ip:port> --listen-addr <ip:port>

# to get the token or command needed to add workers or managers
docker swarm join-token <worker|manager>

# on the workers run the command (remember the manager port should be forwarded
# and not firewalled)
docker swarm join --advertise-addr ip:port --token <token> ip:port

```


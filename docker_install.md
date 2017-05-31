# Docker Install

1. ## update docker engine

> $ uname -r
> 3.11.0-15-generic



> $ apt-get update
> $ apt-get install apt-transport-https ca-certificates
> $ apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D



> $ vi /etc/apt/sources.list.d/docker.list
> 替换为：
> deb https://apt.dockerproject.org/repo ubuntu-trusty main



> $ apt-get update
> $ apt-get purge lxc-docker
> $ apt-cache policy docker-engine



> $ apt-get update
> $ apt-get install linux-image-extra-$(uname -r)



> $ apt-get update
> $ apt-get install docker-engine
> $ service docker start
> $ docker run hello-world



Upgrade Docker

> $ apt-get update
> $ apt-get upgrade docker-engine



使用阿里云的docker镜像加速

> $ echo "DOCKER_OPTS=\"--registry-mirror=https://yeyieqis.mirror.aliyuncs.com\"" | sudo tee -a /etc/default/docker
> $ service docker restart



Install Docker Compose

查看当前Compose最新版本
https://github.com/docker/compose/releases

> $ apt-get install python-pip
> $ pip install docker-compose

或者

> $ apt-get install curl
> $ curl -L https://github.com/docker/compose/releases/download/1.7.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
> $ chmod +x /usr/local/bin/docker-compose

更新方法

> $ pip install docker-compose --upgrade



参考：

- https://docs.docker.com/engine/installation/linux/


- https://docs.docker.com/engine/installation/linux/ubuntulinux/

- https://docs.docker.com/compose/install/ 

- https://github.com/docker/compose

- [Docker 中文指南]: http://docker.widuu.com/






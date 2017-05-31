# GitLab-CI Install



## 安装GitLab Runner

#### 使用docker-compose方式安装

docker-compose.yml

> version: '2'
> services:  
>   GitlabCIMultiRunner:    
> ​    restart: always    
> ​    image: sameersbn/gitlab-ci-multi-runner:1.1.4    
> ​    volumes:    
> ​    -/srv/docker/gitlab-runner:/home/gitlab_ci_multi_runner/data    
> ​    environment:   
> ​    -CI_SERVER_URL=http://gitci.gionee.com:10080/ci  
> ​    -RUNNER_TOKEN=b4u96hH3UsTkQi_sVXxy －》这个参数从你配置的runner界面获提
> ​    -RUNNER_DESCRIPTION=myrunne
> ​    -RUNNER_EXECUTOR=shell

运行该容器后，刷新http://120.26.104.146:10080/tester/utci/runners 可以见一个可使用的runner

#### 更换runner

虽然通过docker-compose down卸载了原来的runner容器，但因为配置:

> -/srv/docker/gitlab-runner:/home/gitlab_ci_multi_runner/data

其中/srv/docker/gitlab-runner会残留一个config.toml配置文件，所以要更换新的runner，要不就删除这个配置文件，要不就使用另外一个新目录进行映射

> -/srv/docker/gitlab-runner**/xxxx**:/home/gitlab_ci_multi_runner/data



参考 sameersbn/docker-gitlab-ci-multi-runner 

[ameersbn/docker-gitlab-ci-multi-runner]: https://github.com/sameersbn/docker-gitlab-ci-multi-runner
[docker-gitlab-ci-multi-runner]: https://github.com/sameersbn/docker-gitlab-ci-multi-runner/blob/master/docker-compose.yml

## 创建已安装的JAVA和MAVEN的RUNNER容器

#### 参考这两个Dockerfile文档

> [docker-maven]: https://github.com/carlossg/docker-maven/blob/40cbcd2edc2719c64062af39baac6ae38d0becf9/jdk-8/Dockerfile
> [ubuntu-openjdk-8-jdk]: https://hub.docker.com/r/picoded/ubuntu-openjdk-8-jdk/~/dockerfile/

#### 创建一个带有java和maven安装的Dockerfile文件

```
FROM sameersbn/gitlab-ci-multi-runner:1.1.4
MAINTAINER chenjunbiao <chenjunbiao@outlook.com>

# Install the python script required for "add-apt-repository"
RUN apt-get update && apt-get install -y software-properties-common

# Sets language to UTF8 : this works in pretty much all cases
ENV LANG en_US.UTF-8
RUN locale-gen $LANG

# Setup the openjdk 8 repo
RUN add-apt-repository ppa:openjdk-r/ppa

# Install java8
RUN apt-get update && apt-get install -y openjdk-8-jdk

# Setup JAVA_HOME, this is useful for docker commandline
#RUN apt-get install -y oracle-java8-set-default
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME


ENV MAVEN_VERSION 3.3.9

RUN mkdir -p /usr/share/maven \
  && curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
    | tar -xzC /usr/share/maven --strip-components=1 \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven

VOLUME /root/.m2
```

#### 创建一个新的docker images

```
docker build -t="chenjunbiao/gitlab-ci-multi-runner-java:1.1.4" .
```



## 创建已安装的JAVA和MAVEN的RUNNER容器

#### 创建一个带有java和ant安装的Dockerfile文件

```
# Installs Ant
ENV ANT_VERSION 1.9.5
RUN cd && \
    wget -q http://archive.apache.org/dist/ant/binaries/apache-ant-${ANT_VERSION}-bin.tar.gz && \
    tar -xzf apache-ant-${ANT_VERSION}-bin.tar.gz && \
    mv apache-ant-${ANT_VERSION} /opt/ant && \
    rm apache-ant-${ANT_VERSION}-bin.tar.gz
ENV ANT_HOME /opt/ant
ENV PATH ${PATH}:/opt/ant/bin

```



## 上传本地项目到远程仓库

```
git init
git add -A
git commit -m "init project"
git remote add origin http://gitci.gionee.com:10080/tester/com.gionee.utci.mvnjar.git
git pull origin master
git push -u origin master

```



## 配置Configuration of your builds with .gitlab-ci.yml

#### 安装java8

> ```
> $ sudo apt-get install software-properties-common
> $ sudo add-apt-repository ppa:webupd8team/java
> $ sudo apt-get update
> $ sudo apt-get install oracle-java8-installer
> ```


> ```
> $ sudo apt-get install oracle-java8-set-default
> ```

编写到.gitlab-ci.yml的before_script

> before_script:
> -add-apt-repository ppa:webupd8team/java
> -apt-get update -qq && apt-get -y -qq oracle-java8-installer oracle-java8-set-default



[How to Install JAVA 8 (JDK 8u91)]: http://tecadmin.net/install-oracle-java-8-jdk-8-ubuntu-via-ppa/#
[How To Install Java on Ubuntu with Apt-Get]: https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get

#### 修改GitLab Server显示的地址为gitlab.tester

##### vi docker-compose.yml

> environment:    
> -GITLAB_HOST=gitlab.gionee.com

##### 重启容器

> $ docker-compose down
> $ docker-compose up

##### 重新下载代码

> git config --global user.name "chenjunbiao"
> git config --global user.email "chenjunbiao@gionee.com"
> git clone http://gitci.gionee.com:10080/tester/com.gionee.utci.onejar.git

[Available Configuration Parameters]: https://github.com/sameersbn/docker-gitlab#host-uid--gid-mapping

#### .gitlab-ci.yml的继续编写



[]: 
[.gitlab-ci.yml]: http://docs.gitlab.com/ce/ci/yaml/README.html
[Quick Start]: http://docs.gitlab.com/ce/ci/quick_start/README.html







## docker-gitlab-ci-multi-runner

[Configuration of your builds with .gitlab-ci.yml]: http://docs.gitlab.com/ce/ci/yaml/README.html
[Gitlab 部署 CI 持续集成]: http://www.cnblogs.com/xishuai/p/gitlab-ci.html
[sameersbn/gitlab-ci-multi-runner]: https://github.com/sameersbn/docker-gitlab-ci-multi-runner
[docker-gitlab-ci-multi-runner]: https://github.com/sameersbn/docker-gitlab-ci-multi-runner
[gitlab-ci-multi-runner#installation]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner#installation
[Run gitlab-runner in a container]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/install/docker.md
[using_docker_images.html]: http://docs.gitlab.com/ce/ci/docker/using_docker_images.html
[runners]: http://docs.gitlab.com/ce/ci/runners/README.html
[Quick Start]: http://120.26.104.146:10080/help/ci%2Fquick_start/README
[.gitlab-ci.yml]: https://about.gitlab.com/2015/05/06/why-were-replacing-gitlab-ci-jobs-with-gitlab-ci-dot-yml/
[config.toml]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/commands/README.md#configuration-file
[CI User documentation]: http://docs.gitlab.com/ce/ci/
[Configuration of .gitlab-ci.yml]: http://docs.gitlab.com/ce/ci/yaml/README.html
[Using Docker Images]: http://docs.gitlab.com/ce/ci/docker/using_docker_images.html
[docker-gitlab-ci-multi-runner-ruby]: https://github.com/sameersbn/docker-gitlab-ci-multi-runner
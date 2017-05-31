# Dockerfile of android sdk

[webratio/android-sdk]: https://hub.docker.com/r/webratio/android-sdk/~/dockerfile/

### 通过直接拷贝SDK方式

```
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


# Installs Ant
ENV ANT_VERSION 1.9.5
RUN cd && \
    wget -q http://archive.apache.org/dist/ant/binaries/apache-ant-${ANT_VERSION}-bin.tar.gz && \
    tar -xzf apache-ant-${ANT_VERSION}-bin.tar.gz && \
    mv apache-ant-${ANT_VERSION} /opt/ant && \
    rm apache-ant-${ANT_VERSION}-bin.tar.gz
ENV ANT_HOME /opt/ant
ENV PATH ${PATH}:/opt/ant/bin

# create android runtime directory
# docker-compose should add volumes:
# - /opt/ci/gitlab/a1runner/android-sdk-linux_x86:/etc/android-sdk-linux_x86:Z
RUN mkdir -p /etc/android-sdk-linux_x86

# Installs i386 architecture required for running 32 bit Android tools
RUN dpkg --add-architecture i386 && \
    apt-get update -y && \
    apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get autoremove -y && \
    apt-get clean

```








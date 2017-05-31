# Gitlab CI Runner的创建和配置





## 安装 gitlab-ci-multi-runner

#### ubuntu下安装git

```
sudo apt-get install git
```



#### Add GitLab's official repository via apt-get:

```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.deb.sh | sudo bash
```

#### Install `gitlab-ci-multi-runner`:

```
sudo apt-get update
sudo apt-get install gitlab-ci-multi-runner
```

#### Register the runner:

```
sudo gitlab-ci-multi-runner register
```

```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/ci )
http://gitci.gionee.com:10080/ci
Please enter the gitlab-ci token for this runner
gf2mUDT2FNay31g4VgsG
Please enter the gitlab-ci description for this runner
CIRuner
INFO[0034] fcf5c619 Registering runner... succeeded
```

#### 安装服务

```
gitlab-ci-multi-runner install -n "CIRuner"
```

#### 启动服务

```
gitlab-ci-multi-runner start -n "CIRuner"
gitlab-ci-multi-runner stop -n "CIRuner"
gitlab-ci-multi-runner restart -n "CIRuner"
```

#### 验证runner

```
gitlab-ci-multi-runner verify
```



#### 如果遇到翻墙问题

```
wget https://packages.gitlab.com/runner/gitlab-ci-multi-runner/ubuntu/pool/trusty/main/g/gitlab-ci-multi-runner/gitlab-ci-multi-runner_1.3.3_amd64.deb
```



```
dpkg -i gitlab-ci-multi-runner_1.3.3_amd64.deb
```



#### ~~如果遇到更新源问题~~

```
/etc/apt/sources.list /etc/apt/sources.list.bak
vi /etc/apt/sources.list
或者
gedit /etc/apt/sources.list
```

删除并替换为如下内容

```
deb http://mirrors.163.com/ubuntu/ precise main restricted
deb-src http://mirrors.163.com/ubuntu/ precise main restricted
deb http://mirrors.163.com/ubuntu/ precise-updates main restricted
deb-src http://mirrors.163.com/ubuntu/ precise-updates main restricted
deb http://mirrors.163.com/ubuntu/ precise universe
deb-src http://mirrors.163.com/ubuntu/ precise universe
deb http://mirrors.163.com/ubuntu/ precise-updates universe
deb-src http://mirrors.163.com/ubuntu/ precise-updates universe
deb http://mirrors.163.com/ubuntu/ precise multiverse
deb-src http://mirrors.163.com/ubuntu/ precise multiverse
deb http://mirrors.163.com/ubuntu/ precise-updates multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-updates multiverse
deb http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-security main restricted
deb-src http://mirrors.163.com/ubuntu/ precise-security main restricted
deb http://mirrors.163.com/ubuntu/ precise-security universe
deb-src http://mirrors.163.com/ubuntu/ precise-security universe
deb http://mirrors.163.com/ubuntu/ precise-security multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-security multiverse
deb http://extras.ubuntu.com/ubuntu precise main
deb-src http://extras.ubuntu.com/ubuntu precise main
```

更新系统

```
$ apt-get update
$ apt-get dist-upgrade
```



[Gitlab CI Runner的创建和配置]: http://zhaozhiming.github.io/blog/2015/11/30/gitlab-ci-runner-create-and-config/
[gitlab-ci-multi-runner]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner#installation/
[linux-repository.md]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/install/linux-repository.md
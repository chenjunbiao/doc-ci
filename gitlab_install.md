# Install Gitlab By Docker



## 1. docker pull 镜像

> $ docker pull sameersbn/gitlab:8.8.5
>
> $ docker pull sameersbn/postgresql:9.4-22
>
> $ docker pull sameersbn/redis:latest

sameersbn 镜像的参考地址：

- https://github.com/sameersbn/docker-gitlab
- https://github.com/sameersbn/docker-postgresql
- https://github.com/sameersbn/docker-redis



## 2. 安装GitLab

### launch by docker-compose

> $ wget https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml

生成GITLAB_SECRETS_DB_KEY_BASE

> $ apt-get install pwgen
> $ pwgen -Bsv1 64
> L3XggdmtsxsNbmkMVVNKJpgNwRFcv4F3TvpfwFnCCKVWrFcsRRMkqVg9KfPtkF9d

设置docker-compose.yml

> $ vi docker-compose.yml
> GITLAB_SECRETS_DB_KEY_BASE=L3XggdmtsxsNbmkMVVNKJpgNwRFcv4F3TvpfwFnCCKVWrFcsRRMkqVg9KfPtkF9d

[Available Configuration Parameters]: https://github.com/sameersbn/docker-gitlab#available-configuration-parameters
[Compose file reference]: https://docs.docker.com/compose/compose-file/

### email config

> GITLAB_EMAIL=chenjunbiao@gionee.com    
> GITLAB_EMAIL_REPLY_TO=chenjunbiao@gionee.com 
> GITLAB_INCOMING_EMAIL_ADDRESS=chenjunbiao@gionee.com



> SMTP_ENABLED=true   
> SMTP_DOMAIN=smtp.gionee.com   
> SMTP_HOST=smtp.gionee.com   
> SMTP_PORT=25    
> SMTP_USER=account@gionee.com    
> SMTP_PASS=***    
> SMTP_STARTTLS=false
> SMTP_AUTHENTICATION=login



### SSL config

#### Generation of Self Signed Certificates

STEP 1: Create the server private key

> $ openssl genrsa -out gitlab.key 2048

STEP 2: Create the certificate signing request (CSR)

> $ openssl req -new -key gitlab.key -out gitlab.csr

STEP 3: Sign the certificate using the private key and CSR

> openssl x509 -req -days 3650 -in gitlab.csr -signkey gitlab.key -out gitlab.crt

Strengthening the server security

> openssl dhparam -out dhparam.pem 2048

[docker-gitlab#ssl]: https://github.com/sameersbn/docker-gitlab#ssl

#### Installation of the SSL Certificates

[当Data Store为/home/git/data时]: https://github.com/sameersbn/docker-gitlab#data-store

> mkdir -p /srv/docker/gitlab/gitlab/certs
> cp gitlab.key /srv/docker/gitlab/gitlab/certs/
> cp gitlab.crt /srv/docker/gitlab/gitlab/certs/
> cp dhparam.pem /srv/docker/gitlab/gitlab/certs/
> chmod 400 /srv/docker/gitlab/gitlab/certs/gitlab.key

#### Enabling HTTPS support

> GITLAB_HTTPS=true
> SSL_SELF_SIGNED=true



### docker-compose 启动

[compose]: https://docs.docker.com/compose/
[gettingstarted]: https://docs.docker.com/compose/gettingstarted/

start up your application

> $ docker-compose up

run your services in the background

> $ docker-compose up -d
> $ docker-compose ps

run one-off commands for your services.

> $ docker-compose run web env

stop your services

> $ docker-compose stop
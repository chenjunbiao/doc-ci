# Show test result by config apache

## 安装apache容器

```
$ docker pull eboraas/apache
```



## 编写docker-compose.yml文件

```
version: '2'

services:
  web-runner2:
    restart: always
    image: eboraas/apache:latest
    volumes:
    - /srv/docker/gitlab-runner-2/web/:/var/www/html/
    ports:
    - "8080:80"
    - "8443:443"
```

这样可能通过浏览器浏览web目录，如：http://gitci.gionee.com:8080/  

建议端口号使用"runner的前4位数字"，以便于迅速定位，如：
13688148 取值 “- 1368:80"
38328cff   取值 "- 3832:80"  访问 http://gitci.gionee.com:3832/ 
496db7c4 取值 "- 4967:80"
不足4位用0补充，如果出现少于1024数字，则使用"5"+"runner的前4位数字"



## 目录结构

```
/srv/docker/gitlab-runner-2
├── builds
│   └── 13688148
│       └── 0
│           └── tester
│               └── com.gionee.utci.mvnjar
│                   ├── jar
│                   ├── src
│                   └── target
└── web
    ├── cobertura
    │   ├── css
    │   ├── images
    │   └── js
    ├── lib
    └── site
        ├── css
        └── images
            └── logos

```

项目所在目录与web止录有5层相对路径，在maven内配置为

```
<outputDirectory>${basedir}/../../../../../web/site</outputDirectory>
```




















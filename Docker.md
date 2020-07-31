# Docker

## Java 项目 —— Tomcat 服务器的部署

### 启动数据库

```kotlin
docker run --name=mysql -it -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

### 启动 TomCat

```kotlin
  #方式1：解释一下，-d表示后台运行，-p端口映射，前面的8088是外围访问端口（也就是本机ip对外开放的端口），后面8080是docker容器内部的端口
　docker run -d -p 8088:8080 tomcat

　#方式2：加--name tomcat的意思，表示为此容器启一个别名叫tomcat，以后再也不用操作容器id进行关闭、进容器目录什么的，直接把容器ID换成tomcat别名

加上--restart=always　　表示此容器开机启动，只要docker也设置了开机自启，docker不死

　docker run -d -p 8088:8080 --name tomcat --restart=always tomcat
```

### 进入容器

```kotlin
docker exec -it 9fccf0236619 /bin/bash
```

### 项目拷贝

 默认  webapps 下是没东西的，东西存在 webapps.dist 里面，所以我们要把 webapps.dist 拷贝到 webapps 目录下

```kotlin
root@0be1774e1e5e:/usr/local/tomcat# rm -rf webapps
root@0be1774e1e5e:/usr/local/tomcat# mv webapps.dist webapps
```

```kotlin
#(什么是宿主机：自己当前的服务器centOS7称之为宿主机，宿主机上的docker可看作一个容器，也就是docker所在的服务器称为宿主机)

#解释一下：docker cp xxx.war包路径 容器ID:/要复制过去的目录路径（其实还有另一种方法：使用挂载，
#挂载的意思就是在宿主机上解压一个tomcat把这里面的webapps目录映射到docker内的tomcat容器中的webapps目录，这样直接把war包发送到宿主机的tomcat的webapps下面，docker的tomcat的webapps会共用此目录下的文件）

docker cp /usr/local/testJavaProject/test01.war 9fccf0236619:/usr/local/tomcat/webapps
```

### 项目挂载

手动创建 /usr/local/dev/docker-tomcat 目录后挂载

```
docker run -d -p 80:8080 --name tomcat -v /usr/local/dev/docker-tomcat:/usr/local/tomcat/webapps --restart=always tomcat
```

手动将 webapp.dist 转移

```
# 文件删除
rm -rf /home/www/statics/*
cp -ri /home/server/tomcat/* /home/server/test/
```



### 日志查看

```
docker logs -f -t --since="2017-05-31" --tail=10 edu_web_1

--since : 此参数指定了输出日志开始日期，即只输出指定日期之后的日志。

-f : 查看实时日志

-t : 查看日志产生的日期

-tail=10 : 查看最后的10条日志。

edu_web_1 : 容器名称
```

### VIM 的安装

```visual basic
# 校验是否为最新版本
apt-get update
# 安装
apt-get install *
```

## Nginx 的项目部署

```dockerfile
# 查询版本
docker search nginx
# 取最新版本
docker pull nginx:latest
# 查看本地镜像
docker images
# 运行 nginx 并架设代理服务器(默认是 80 端口)
docker run --name nginx-test -p 8080:80 -d nginx
# --name nginx-test：容器名称。
# -p 8080:80： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口。
# -d nginx： 设置容器在在后台一直运行。
```

### 修改配置文件

- 方法一、nginx 的配置文件在容器的 /etc/nginx 目录下

  - 下载 vim 后可以自己定制 nginx.con 文件
  - 定制结束后，先把容器停掉，然后再启动即可

- 方法二、以挂载的方式进行配置 nginx 服务器

  - ```dockerfile
    docker run --name nginx -p 80:80
    -v /home/docker-nginx/nginx.conf:/etc/nginx/nginx.conf 
    -v /home/docker-nginx/log:/var/log/nginx
    -v /home/docker-nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf
    -d nginx
    
    # --name  给你启动的容器起个名字，以后可以使用这个名字启动或者停止容器
    
    # -p 映射端口，将docker宿主机的80端口和容器的80端口进行绑定
    
    # -v 挂载文件用的，第一个-v 表示将你本地的nginx.conf覆盖你要起启动的容器的nginx.conf文件，第二个表示将日志文件进行挂载，就是把nginx服务器的日志写到你docker宿主机的/home/docker-nginx/log/下面
    
    # 第三个-v 表示的和第一个-v意思一样的。
    
    # -d 表示启动的是哪个镜像
    ```
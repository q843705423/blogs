
# docker安装教程


## 一.yum更新
yum update


## 二.加入新的仓库
tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF


## 三.yum安装docker引擎
yum install -y docker-engine


## 四.开启服务
systemctl start docker.service


## 五.开机启动

sudo systemctl enable docker


# docker 常用命令

------------------------------
|-----------|--------------|
|docker image ls| 列举所有的镜像|
|docker ps|    列举所有的容器|
|docker stop a1b2c3d4 | 关闭a1b2c3d4的镜像|
|docker run -t a1b2c3d4 -p 8080:8080|将其从8080端口映射到8080端口，并且运行|
|./mvnw install dockerfile:build|用mvn配合dockerfile进行build，产生镜像|
|./mvnw dockerfile:push|将dockerfile推送到dockerbub|
|docker login|准备登录docker hub|

-p 8437:8080 从docker容器的8080端口映射到真实机器的8080端口
-t  启动虚拟伪终端

指定容器，一般要指定它的tag,如 843705423/travis_test:fisttry

如何上传docker
参考网址 https://ropenscilabs.github.io/r-docker-tutorial/04-Dockerhub.html

## 如何 用 docker 运行 或 打包 SpringBoot 项目
### 1. 使用 idea生成springBoot的项目
这样它会产生一个mvnw文件，利用该文件，和各种命令，可以对它进行操作

### 2.给生成的SpringBoot的项目的 pom.xml中，添加以下代码
```
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin> <!-- tag::plugin[] -->
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>dockerfile-maven-plugin</artifactId>
                <version>1.4.9</version>
                <configuration>
                    <repository>hello/${project.artifactId}</repository>
                </configuration>
            </plugin> <!-- end::plugin[] --> <!-- tag::unpack[] -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>unpack</id>
                        <phase>package</phase>
                        <goals>
                            <goal>unpack</goal>
                        </goals>
                        <configuration>
                            <artifactItems>
                                <artifactItem>
                                    <groupId>${project.groupId}</groupId>
                                    <artifactId>${project.artifactId}</artifactId>
                                    <version>${project.version}</version>
                                </artifactItem>
                            </artifactItems>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```


```
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <!-- tag::plugin[] -->
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>dockerfile-maven-plugin</artifactId>
                <version>1.4.9</version>
                <configuration>
                    <repository>${docker.image.prefix}/${project.artifactId}</repository>
                </configuration>
            </plugin>
            <!-- end::plugin[] -->

            <!-- tag::unpack[] -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>unpack</id>
                        <phase>package</phase>
                        <goals>
                            <goal>unpack</goal>
                        </goals>
                        <configuration>
                            <artifactItems>
                                <artifactItem>
                                    <groupId>${project.groupId}</groupId>
                                    <artifactId>${project.artifactId}</artifactId>
                                    <version>${project.version}</version>
                                </artifactItem>
                            </artifactItems>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <!-- end::unpack[] -->
        </plugins>
    </build>

```
该代码将引入 dockerfile的插件

### 3. 创建 一个 Dockerfile 在 项目 的 根目录 下
内容如下
```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG DEPENDENCY=target/dependency
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY ${DEPENDENCY}/META-INF /app/META-INF
COPY ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","com.teradata.connect_remote_mysql.ConnectRemoteMysqlApplication"]

```

### 4.运行 以下 代码 准备 生成 该 SpringBoot 项目 的 镜像
```
mvnw install dockerfile:build
```
>> 如果你被报错 Please set the JAVA_HOME variable in your environment to match the location of your Java installation.
那么就要去环境变量里设置自己的JAVA_HOME

## 如何 用 docker 运行 mysql 项目

###  拉取5.7的docker镜像
```
docker pull mysql/mysql-server:5.7
```


###  docker 运行 mysql ,设置容器名为cqq_mysql 初始密码为root 端口为3306
```
docker run --name cqq_mysql -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 mysql:5.7
docker run --name cqq_mysql -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 mysql/mysql-server:5.7
docker run --name cqq_mysql -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306  181f3fa4ea7d
```
  

进入docker的运行终端
```
docker exec -it  cqq_mysql bash
```

如何用docker 创建mysql镜像，并运行？

参考 
https://github.com/docker-library/mysql/issues/275

1.第一步
创建mysqld.cnf文件，里面填充内容如下
```
[mysqld]
pid-file	            = /var/run/mysqld/mysqld.pid
socket		            = /var/run/mysqld/mysqld.sock
# Where the database files are stored inside the container
datadir		            = /var/lib/mysql

# My application special configuration
max_allowed_packet          = 32M
sql-mode                    = 'STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION'

# Accept connections from any IP address
bind-address	            = 0.0.0.0
```

2.第二步
创建Dockerfile，内容如下
```
FROM mysql:5.6
MAINTAINER lucile-sticky "aaa@bbb.ccc"

# Setup the custom configuration
ADD mysqld.cnf /etc/mysql/mysql.conf.d/mysqld.cnf
```

3.  执行命令,生成mysql的docker镜像
```
docker build .
```

4. 生成一个名为mysql2333333333的容器运行刚刚生成的镜像
```
docker run --name mysql2333333333 -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 mysql:5.6
```

5. 进入容器的模拟终端,通过bash的方式
```
docker exec -it mysql2333333333 bash
```

6. 在模拟终端中输入如下代码回车进入数据库
```
mysql -uroot -proot
```

7. 查询有哪些数据库
```
show databases;
```

8. 使用 navicat 来连接数据库


9. 演示完毕,谢谢观看

docker run -p 8080:8080 --name SpringBootProject --link mysql56:mysql:5.6 e97e
docker run -it -d -p 3306:3306 --name localhost  -e MYSQL_ROOT_PASSWORD=root --net bridge mysql:5.6
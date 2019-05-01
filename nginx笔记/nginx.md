# Nginx

### 提供强大的服务
1.静态资源服务
    + 通过本地文件系统提供服务
2.反向代理服务
    + Nginx的强大性能
    + 缓存
    + 负载均衡
3.API服务
    + openResty

当一个请求来的时候，Nginx
很多应用服务需要动态扩容
又需要容灾
Nginx 是内部服务器的边缘的节点
由Nginx可以提供缓存
css，javascript,可以直接由静态资源提供就可以了

当Nginx可以直接访问数据库等


### Nginx的出现的历史背景和原因

+ 互联网的快速普及
+ 全球化
+ 物联网

Apache 一个连接对应一个进程,一个进程只能服务一个连接
进程间切换引发巨大的消耗
Nginx可以处理数百万的连接


### Nginx的使用优点

+ 高并发，高性能
+ 可拓展性好
+ 高可靠性
+ 热部署
+  BSD许可证

32核，64G内存，可以轻松达到上千万的连接
静态的资源请求  100W的请求

模块化设计非常稳定
生态圈丰富
高可用性

企业内网的边缘节点


在不停止服务的上，升级连接


### Nginx的组成部分

汽车
Nginx二进制可执行文件(汽车本身)
Nginx的配置文件:Nginx.conf(驾驶员)
access.log(访问日志,GPS轨迹,运维的分析)
error.log(错误日志,未知错误)

+ feature
+ bugfix
+ change

阿里巴巴 Tengine 修改了 Nginx的主干版本,
OpenResty

### 编译Nginx

01.下载Nginx
02. 介绍各个目录
03. Configure
04. 中间文件
05. 安装nginx

#### 下载
进入官网 nginx.org
```
wget http://nginx.org/download/nginx-1.14.2.tar.gz
tar -xzf nginx.xxx.xxx.tar.gz
cd nginx.xxx.xxx

```

#### 目录结构查看
+ auto
    + cc  (用于编译)
    + CHANGE (版本的变化，及bug)
    + conf (示例的配置文件)
    + configure (脚本:生成编译文件，必备动作)
    + contrib (Vim的配置,可以让Vim有色彩变化)
    + html (html模板文件)
    + man (linux下的配置文件,基本nginx的帮助和配置)
    + src (nginx源代码)
使用这行命令可以让vim具有配色
cp -r contrib/vim/* ~/.vim/

#### nginx编译

编译前查看支持哪些参数
./configure --help | more

其中分为几大块
+ 模块路径查找(一般在prefix建立相应文件夹)
+ with打头默认不编译,加了移动进去
+ without打头默认编译，加了移出去



将Nginx安装到/home/chenqiqi/nginx的目录下
./configure --prefix=/home/chenqiqi/nginx
生成许多中间文件，会放在 objs模块下
ngx_modules.c 显示所有编辑进去的模块
```
make
make install
```

Nginx配置文件ask文件，
+ 有两部分组成，一个是指令，一个是指令块
+ 每条指令以;分号分隔，指令和参数与空格符分隔
+ 指令块以{}组织起来
+ include语句允许组合多个配置文件，以提高可维护性
+ 使用#作为注释，提高可读性
+ 使用$作为变量
+ 部分支持正则

时间单位
ms milliseconds
s  second
m minutes
h hours
d days
w weeks
M months,30days
y year,365 days

空间单位
   bytes
k/K kilobytes
m/M megabytes
g/G gigabytes


http 表示里面都是由http模块去执行的
upstream 指的是上游服务(Tomcat等等)
server 是域名
location 表达式

当我们安装好nginx后，可以启动服务
nginx -s reload
-? -h 帮助
-c 指定配置文件
-g 指定配置指令
-p 指定运行目录
-s 发送信号
+ stop
+ quit(优雅停止服务)
+ reload(重新加载配置文件)
+ reopen(换日志文件)
-t (测试配置文件)


进入
conf/nginx.conf 改变部分的值

不中断的重载配置文件
./nginx -s reload

如果更新nginx，那么需要先备份sbin/nginx
cp nginx nginx.old
替换掉进程所使用的nginx文件
cp -r nginx /usr/local/openrestty/nginx/sbin/ -f

kill -USR2 13195
ps -ef | grep nginx
kill -WINCH 13195


--------------------------------------
日志切割

mv taohui_access.log bak.log
../sbin/nginx -s reopen  切换日志

把这个脚本放在  rotate.sh


root与 alias 在location中有各种区别
```
server{
    listen 8080;
    location / {
        alias hello/;
    }
}
```


```
gzip可以进行压缩，来减小文件的大小
```
../sbin/nginx -s reload

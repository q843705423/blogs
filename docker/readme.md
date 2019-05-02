

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

## 
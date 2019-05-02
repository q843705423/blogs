
## 安装jre
sudo yum install java-1.8.0-openjdk

## 安装jdk
sudo yum install java-1.8.0-openjdk-devel


vim  /etc/profile

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
export CALSSPATH=$JAVA_HOME/lib/*.*
export PATH=$PATH:$JAVA_HOME/bin 



source  /etc/profile

source  /etc/profile
# 在单项目下的配置
SpringBoot项目中，如何快速切换其使用的配置文件?

这里有许多方式
## 在一个主配置文件中进行配置
如果在resources文件夹下有

+ application.yml
+ application-dev.yml
+ application-prod.yml

那么在application.yml中指定spring.profiles.active=dev/prod
可以切换指定所使用的配置文件
## 使用启动配置项
当然，我们也可以不使用application.yml

在IDEA中Edit Configurations里的VM option=-Dspring.profiles.active=dev/prod
也可以达到切换的效果

## 在打包成jar时,可以指定运行参数
```
java -jar my.jar --spring.profiles.active=dev
```
当项目被打包成jar的时候，通过这种方式，就可以指定不同的配置文件

# 分布式环境下的配置
那么如何在集群配置中，并且含有eureka集群，以及一个配置中心集群的时候 ，指定一个配置中?

分布式的环境下，
每个模块会根据自己的yml文件去找到eureka中心，
然后再找到配置中心
在配置中心下，以一定的优先级去找到自己所属模块的基本配置


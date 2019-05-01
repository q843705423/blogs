# 传统的ACID

## ACID即为

+ A(Atomicity)原子性
+ C(Consistency)一致性
+ I(Isolation)独立性
+ D(Durability)持久性

## CAP即为

+ C(Consistency)强一致性
+ A(Availability)高可用性
+ P(Partition tolerance)分区容错性

任何分布式系统只能满足其中2个
即为CAP的3进2原则

需要满足高可用，而抛弃强一致

Eureka 是AP
而zookeeper是CP

Nginx Ribbon feign

Ribbon有90%就是nginx
Ribbon是客户端负载均衡工具

LB load balance
+ 集中式LB
+ 进程类LB


eureka

ribbon与eureka整合后，
不需要再关心服务的ip的port

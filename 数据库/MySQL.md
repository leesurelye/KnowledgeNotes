# MySQL架构

## MySQL结构图

![image-20220907154621851](../.image/image-20220907154621851.png)

# 分布式锁的实现方式
1. 可以使用数据库的乐观锁实现
2. 使用redis的setnx()、expire()方法，用于分布式锁
3. 基于Zookeeper实现分布式锁
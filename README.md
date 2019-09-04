docker-compose负责实现对Docker容器集群的快速编排。

1、安装python-pip
yum -y install epel-release
yum -y install python-pip

2、安装docker-compose
pip install docker-compose && docker-compose version

3、运行命令
docker-compose up -d


4、看容器内网的 ip 地址 
docker inspect master
或者
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' master 


5、配置 Sentinel 哨兵

进入 3 台 redis 容器内部进行配置，在容器根目录里面创建 sentinel.conf 文件
注意：！！例为（172.17.0.2）master ip 
文件内容为：sentinel monitor mymaster 172.17.0.2 6379 1

docker exec -it slave1 /bin/bash 
cd / && touch sentinel.conf

1、apt-get update    2、apt-get install vim -y

vim /sentinel.conf

最后，启动 Redis 哨兵：
redis-sentinel /sentinel.conf

Sentinel 哨兵配置完毕。 (是所有 redis 都要运行哦)

2、测试 关闭 Master

docker stop master

这时，剩余的 2 个从机，会自动选举产生新的主机，这里选举 172.17.0.3 为主机。


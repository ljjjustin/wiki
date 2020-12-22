<link rel="stylesheet" type="text/css" href="/auto-number.css">

# Redis架构剖析

## 进程模型


## 高可用

### 哨兵模式
通过哨兵来实现高可用时，应用程序首先与哨兵建立连接，通过哨兵获取redis的主节点，然后再连接到主节点。
正常运行期间，哨兵会周期性的检查redis的运行状态，如果节点在检测周期中没有响应，会把哨兵标记为主观离线，
然后询问其他哨兵，如果超过半数哨兵认为改节点离线，就会执行离线处理。如果要离线的节点是主节点，会触发主备切换。
哨兵会选举一个从节点为新的主节点，并自动修改主从关系。

### 集群模式

## 数据持久化

Redis有两种方法实现数据的持久化，一种是基于内存快照，另一种是数据直接落盘。

### RBD

RBD是基于内存快照，通过`save`或`bgsave`触发。执行save之后，redis首先会创建一个子进程，然后通过子进程将数据保存到磁盘上；主进程则继续接受读写请求。。

### AOF


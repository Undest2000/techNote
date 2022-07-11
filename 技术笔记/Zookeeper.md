## Zookeeper

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-28-10-34-28-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-28-10-34-51-image.png)

![](C:\Users\fengying\AppData\Roaming\marktext\images\2022-06-28-10-35-18-image.png)

一旦ZooKeeper集合启动，它将等待客户端连接。客户端将连接到ZooKeeper集合中的一个节点。它可以是领导或跟随者节点。一旦客户端被连接，节点将向特定客户端分配会话ID并向该客户端发送确认。如果客户端没有收到确认，它将尝试连接ZooKeeper集合中的另一个节点。 一旦连接到节点，客户端将以有规律的间隔向节点发送心跳，以确保连接不会丢失。

- **如果客户端想要读取特定的znode，**它将会向具有znode路径的节点发送**读取请求**，并且节点通过从其自己的数据库获取来返回所请求的znode。为此，在ZooKeeper集合中读取速度快。

- **如果客户端想要将数据存储在ZooKeeper集合中**，则会将znode路径和数据发送到服务器。连接的服务器将该请求转发给领导者，然后领导者将向所有的跟随着重新发出写入请求。如果只有大部分节点成功响应，而写入请求成功，则成功返回代码将被发送到客户端。 否则，写入请求失败。绝大多数节点被称为 **Quorum** 。

配置**conf/zoo.cfg**

tickTime = 2000
dataDir = /path/to/zookeeper/data
clientPort = 2181
initLimit = 5
syncLimit = 2

启动 $ bin/zkServer.sh start

客户端：$ bin/zkCli.sh

停止：$ bin/zkServer.sh stop

创建普通默认持久节点

create /path /data  

create /FirstZnode “Myfirstzookeeper-app"

创建顺序节点-s

create -s /path /data

创建临时节点-e

create -e /path /data

获取数据

get /path

监视节点

get /path [watch] 1

设置节点

set /path /data

列出子节点

ls /path

检查状态

stat /path

移除节点及子节点

rmr /path

移除没有子节点的节点

delete /path

## Java绑定

让我们来了解本章中最重要的一组ZooKeeper API。ZooKeeper API的核心部分是**ZooKeeper类**。它提供了在其构造函数中连接ZooKeeper集合的选项，并具有以下方法：

- **connect** - 连接到ZooKeeper集合

- ****create****- 创建znode

- ****exists****- 检查znode是否存在及其信息

- **getData** - 从特定的znode获取数据

- **setData** - 在特定的znode中设置数据

- **getChildren** - 获取特定znode中的所有子节点

- **delete** - 删除特定的znode,只能删没有叶子节点的节点

- ****close**** - 关闭连接

rmr自定义方法：

public  static  void  rmr(String  path , ZooKeeper zk) throws Exception {
        //  /a/b   /a/c/d
        // 判断是否有子节点   递归删除
        List<String> ls = zk.getChildren(path, null);
        if(ls!=null && ls.size()>0){ // 有子节点
            // 遍历所有的子节点
            for (String name : ls) {
                rmr(path+"/"+name , zk) ;
            }
        }
        // 删除当前叶子节点
        zk.delete(path , -1);
    }

## 本质上来说zookeeper就是用来同步多台计算机之间的数据，存在内存的数据库中（一颗树结构），一段时间会把它们存在磁盘上。

dubbo用zookeeper作为注册中心就保证了如果你向一台装有zookeeper的电脑请求rfc服务，它挂掉了，其他装有zookeeper和dubbo的电脑照样可以完成这个调用

activemq也是同样的道理，如果你向一台装有zookeeper的电脑请求消费消息，它挂掉了，其他装有zookeeper和activemq的电脑照样可以完成这个调用

zookeeper集群搭建:

[zookeeper集群环境搭建详细图文教程 - 小周JAVA技术分享 - 博客园](https://www.cnblogs.com/zhoubang521/p/8178997.html)

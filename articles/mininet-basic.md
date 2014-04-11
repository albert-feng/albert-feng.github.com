
SDN是目前一个非常火热的技术，重新定义了数据中心的网络技术。不过这种技术在学习的时候会稍微有点麻烦，缺少足够的物理设备，不过[mininet](http://mininet.org/)提供了一个非常好的方法来模拟一个SDN网络里面的各种设备。

mininet的安装
----
mininet官方提供了3种安装方法，在这里我使用源码安装的方式，其他的安装方法可以见[官方文档](http://mininet.org/download/)
使用源码安装的时候官方推荐使用Ubuntu，因为它对于Open vSwitch有着更好的支持。第一步是下载源码
```shell
git clone git://github.com/mininet/mininet
```
下载好之后就可以执行安装的命令了：
```shell
mininet/util/install.sh [options]
```
option包含3个选项
* -a：安装所有的东西，包括mininet，Open vSwitch，OpenFlow，wireshark，dissector和POX。
* -nfv：安装mininet，Open Flow和Open vSwitch
* -s mydir：使用选择的路径来存放代码进行安装（安装过程中会下载一些代码）

mininet的基本使用
----
安装好之后，就可以开始使用mininet来进行一些基本的操作了。
首先来进入mininet的CLI，因为执行mininet命令的时候需要root权限，所以直接用root账户来操作比较方便。
```shell
$ sudo mn
```
输入命令后，会进入mininet的CLI。并且创建出一个默认的拓扑结构，包含一个支持OpenFlow的交换机，两个主机和一个OpenFlow Controller。如果想实现出不同的拓扑，可以使用--topo选项来实现更多的功能。
展示出mininet CLI的帮助文档
```shell
mininet> help
```
查看拓扑里的所有节点
```shell
mininet> nodes
```
查看拓扑的网络连接
```shell
mininet> net
```
查看拓扑里的主机，交换机和Controller的详细信息
```shell
mininet> dump
```
查看主机的网络设置
```shell
mininet> h1 ifconfig -a
```
测试两个主机之间的连通性
这里的命令其实就跟linux上的一下操作非常类似了，了解linux的话很快能够上手
```shell
mininet> h1 ping -c 1 h2
```
还可以在主机上启动一个HTTP Server
```shell
mininet> h1 python -m SimpleHTTPServer 80 &
mininet> h1 netstat -anp|grep 80
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      3222/python
```
在你执行完需要的操作后，你就可以来清除mininet
```shell
$ sudo mn -c
```
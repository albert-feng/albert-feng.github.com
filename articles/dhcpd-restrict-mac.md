
前几天解决了个很奇葩的DHCP的问题，在这记录下解决的方法。
之前的分析过程就不赘述了，DHCP的原理见[wiki](http://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E4%B8%BB%E6%9C%BA%E8%AE%BE%E7%BD%AE%E5%8D%8F%E8%AE%AE)，总之到最后，分析结果就是需要让DHCP Server需要把'c6:b0'开头的mac地址发过来的DHCP discover请求拒绝掉。DHCP Server使用的是Dhcpd。

在Dhcpd的[man](http://linux.die.net/man/5/dhcpd.conf)里面，终于找到了解决的方法。

Client Classing
----
在Dhcpd的文档里，介绍了一种方法来定义一个类，用来对访问DHCP服务的客户端进行分类，这就叫Client Classing。具体定义的方法就跟面向对象的编程语言里面定义一个类非常类似。

```shell
class "clients" {
  match if substring (option address, 1, 2) = "c6:b0";
}
```

上面的代码里定义了一个叫client的类，它会匹配mac地址开头为c6:b0的DHCP client。这样就把我想要区分的DHCP client区别出来了。

Deny
----
把相应的client区分出来之后，接下来就可以看如何来拒绝这些client发出的DHCP请求了。
在Dhcpd的配置文件里支持对已定义的类进行拒绝。

```shell
members of "class";
```

那么结合前面的定义，就应该这么写：

```shell
members of "clients";
```
那么这样就能够对于特定的mac地址开头的设备发送的DHCP请求进行拒绝了。

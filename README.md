rinetd, by Thomas Boutell. Released under the terms of the GNU General
Public License, version 2 or later.

This program is used to efficiently redirect connections from one IP
address/port combination to another. It is useful when operating virtual
servers, firewalls and the like.

To build under Unix, run "./bootstrap" to create the configuration
files, then "./configure" to create the build files, and then type
"make" to build rinetd. To install, type "make install" as root.

For documentation run "make install", then type "man rinetd" for
details. Or, read index.html in your browser.


###install
```shell
git clone https://github.com/samhocevar/rinetd.git
cd rinetd
#wget https://github.com/samhocevar/rinetd/archive/master.zip && unzip master.zip && cd master

sh bootstrap
./configure --sysconfdir=/etc/ --bindir=/usr/bin/ --sbindir=/usr/bin/
make && make install
```


###use age

Most entries in the configuration file are forwarding rules. The format of a forwarding rule is as follows:

bindaddress bindport connectaddress connectport
For example:
206.125.69.81 80 10.1.1.2 80
Would redirect all connections to port 80 of the "real" IP address 206.125.69.81, which could be a virtual interface, through rinetd to port 80 of the address 10.1.1.2, which would typically be a machine on the inside of a firewall which has no direct routing to the outside world.
Although responding on individual interfaces rather than on all interfaces is one of rinetd's primary features, sometimes it is preferable to respond on all IP addresses that belong to the server. In this situation, the special IP address 0.0.0.0 can be used. For example:

0.0.0.0 23 10.1.1.2 23
Would redirect all connections to port 23, for all IP addresses assigned to the server. This is the default behavior for most other programs.
Service names can be specified instead of port numbers. On most systems, service names are defined in the file /etc/services.

Both IP addresses and hostnames are accepted for bindaddress and connectaddress.

```conf
[bindaddress] [bindport] [connectaddress] [connectport]
绑定的地址    绑定的端口  连接的地址      连接的端口

[Source Address] [Source Port] [Destination Address] [Destination Port]
源地址            源端口         目的地址               目的端口
```


```shell
$ vim /etc/rinetd.conf

0.0.0.0 8080 172.19.94.3 8080
0.0.0.0 2222 192.168.0.103 3389
1.2.3.4 80 192.168.0.10 80
allow *.*.*.*
logfile /var/log/rinetd.log
```


```shell
/usr/bin/rinetd
/usr/bin/rinetd -c /etc/rinetd.conf
/usr/bin/rinetd stop

```

###notice
>1.rinetd.conf中绑定的本机端口必须没有被其它程序占用
>2.运行rinetd的系统防火墙应该打开绑定的本机端口
>3.不支持FTP的跳转

### TCP/IP协议族体系结构及主要协议

#### 数据链路层

常用协议：ARP协议（Address Resolve Protocol，地址解析协议）和RARP协议（Reverse Address Resolve Protocol，逆地址解析协议，通过MAC地址映射IP地址）

协议的功能：实现IP地址和MAC地址的相互转换

数据链路层使用物理地址寻址一台机器

范围：局域网

#### 网络层

常用协议：IP协议（Internet Protocol，因特网协议）及ICMP协议（Internet Control Message Protocol，因特网控制报文协议）

协议的功能：根据数据包的目的IP地址决定投递给哪一个路由器，并由路由器进行转发，如果不能直接发送给目标主机，就通过寻找合适的下一跳路由器进行转发

网络层实现数据包的选路和转发

范围：广域网

此处可联系现实，对于家用路由器来说，它给pc分配的是私人IP，数据包从私人IP：端口发出，在路由器内建立私网IP->公网地址：端口的映射关系（NAT），数据包进入公网；回包时到达路由器查表还原，转发回内网设备。

#### 传输层

常用协议：TCP协议（Transmission Control Protocol，传输控制协议），UDP协议（User Datagram Protocol，用户数据报协议），SCTP协议（Stream Control Transmission Protocol，流控制传输协议）

协议的功能：

传输层为两台主机上的应用程序提供端到端的通信

#### 应用层










### TCP ip 协议

### 链接过程  
1. 一些半链接的状态


### 窗口的流量控制 

### 窗口拥塞控制 


https://www.nowcoder.com/discuss/1937 
### 考点
### TCP 四次挥手过程
### TIME_WAIT 等待两个 MSL 作用（两个）
1. 如果对面还没收到ACK，那么服务器就会重发 FIN，如果服务器就提取关闭 那么客户端将响应RST分节，服务器将此分节解释成一个错误 马上又重新建立起一个相同的IP地址和端口之间的TCP连接，后一个连接被称为前一个连接的化身 （incarnation)，那么有可能出现这种情况，前一个连接的迷途重复分组在前一个连接终止后出现，从而被误解成从属于新的化身。
### 如果在 TIME_WAIT等待时客户端手动指定该端口再次发起连接，那么可以建立连接吗？
不能
### 客户端收到一个窗口为 0 的包怎么处理
https://www.zhihu.com/question/57856855
1. 启动定时器 
如果B数据发送新的数据，且B.WINDOWS>0 
关闭PT
有数据则发送
更新自己的串口
+ 如果定期到期 ，尝试发送1个字节，尝试
+ 刷新定时器
+ 如果超过一定的次数 则发送reset

### 讲一下 TCP 拥塞控制算法
1. 慢启动 

### TCP 三次握手，服务端回复时可以将 ACK 和SYN分开发送吗（考察四次握手？）
可以
### Linux 如何抓包
TCPdump 
### TCP 与 UDP 区别
### TCP 可靠机制保证
### 下TCP的报头

是否用过 select、epoll，什么是IO多路复用，epoll 水平触发和边缘触发

### 连接重置可能是因为什么原因造成的？
https://my.oschina.net/costaxu/blog/127394
+ 请求超时 
+ 打开不可打开端口 
+ 提前关闭 应用程序不接受
+ 已关闭的socket上收到数据 

网络通信中管道选取异常什么情况下会出现？

lsof、tcpdump、gdb、Nginx

tcpdump

RST知道吧，发送一个RST会发生什么
+ RST 是表示链

5.tcp粘包听说过吗 

6.tcp怎么实现拥塞控制 

7.tcp怎么保证可靠传输

 9.udp可以使用connect吗
https://www.zhihu.com/question/24853633 
 TCP握手以及每一次握手客户端和服务器端处于哪个状态（11种状态）
 server -> lisen -> Reciver_SYN-> Establish 
 client -> Send_SYN-> Establish 
 发送三次的原因  如果两者相互确定序列号，如果只是二次我是，那么久无法确定两者序号是否同步。 ACK不会重传。
 exchange 
 client 主动关系  -> FIN_WAINT_I->接收到 ACK-> 变为FIN_WAIT_II -> 接收到FIN信号->TIME_WAIT ACK 
 server 被动 -> close_WAIT 表示发出了ACK报文 -> 发出FIN信号 -> 接收到ACK
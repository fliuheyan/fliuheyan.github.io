## network

2. Data Link (mac)          arping
3. Network (IP)             ping, traceroute, tracepath
4. Transport (TCP)- blocked ports, firewalls         ss, telnet, tcpdump,netcat  
5-7 application                                      dig


### commands

#### ip route

#### tcpdump

### 性能测试

1. 带宽

`patcher`

`bing`

2. 吞吐量
可提供最大容量
`ttcp`

3. 数据流测量
实际使用


### TCP/IP


#### 三次握手

#### 滑动窗口

#### 重复ACK

#### 快速重传

### Q&A

1. PSH 标志位

2. Wireshark用法 (filter)

3. TCP报文段大小设置???

4. 为什么需要平静时间 ? 
iss序号会绕回，所以新建立连接的iss序号可能接近原有连接最后使用的iss序号.
解决方法就是重启之后在msl时间内不发送任何报文.

5. msl时间内server端会做什么？

6. 粘包和分包出现的原因
tcp记录报文时候不是按照报文号来记录的，而是按照报文字节数记录的.(0:1024)

7. UDP 和 TCP
UDP流式协议，没有

8. 跨越get post 需要option

9. 

### 常用命令

1. `nmcli` 

`nmcli c` 显示device

`nmcli d show device` 显示dns, gateway, mtu等信息


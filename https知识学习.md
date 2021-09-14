> 本文中 https 的学习分为两个大的章节：
>  1. https 数据交互具体过程
>  2. https 发展历程

## https 数据交互具体过程

以访问 https://www.baidu.com 为例，数据包交互过程 follow 如下： 
![https 交互抓包](https://github.com/Saitoler/Network/blob/master/pics/https%E4%BA%A4%E4%BA%92%E6%8A%93%E5%8C%85.png)


详细过程解析如下:  
> 鉴于 https 即 http + SSL/TLS ，故本质上还是使用 TCP 协议传输的， 这里在描述时， 忽略掉 TCP 三次握手的过程，仅对 TLS 交互过程进行解析  

![ssl协商数据包](https://github.com/Saitoler/Network/blob/master/pics/SSL%20%E5%8D%8F%E5%95%86%E4%BA%A4%E4%BA%92%E8%BF%87%E7%A8%8B.jpg)

### 步骤1
客户端发送  Client Hello 报文开始 SSL 通信，报文中包含：  
	- 客户端支持的 SSL 的指定版本  
	- 加密组件列表， 如下图所示
	- 随机数 Random_c, 用于生成后续对称密钥交互过程中用于加密的 session keys
	![client hello](https://github.com/Saitoler/Network/blob/master/pics/client_hello%E6%95%B0%E6%8D%AE%E5%8C%85.png)



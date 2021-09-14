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
> 	- 客户端支持的 SSL 的指定版本  
> 	- 加密组件列表， 如下图所示  
> 	- 随机数 Random_c, 用于生成后续对称密钥交互过程中用于加密的 session keys

![client hello](https://github.com/Saitoler/Network/blob/master/pics/client_hello%E6%95%B0%E6%8D%AE%E5%8C%85.png)

> 	- 在扩展字段中，有一个字段是  server_name 字段， Server Name Indication extension 会包含要访问的具体的 server name 的域名信息：  
![client hello 报文的 server name 扩展字段](https://github.com/Saitoler/Network/blob/master/pics/client_hello_servername.png)

### 步骤2
服务器若可进行 SSL 通信时，会以 Server Hello 报文作为应答，报文中包含：  
> 	- SSL 版本信息  
> 	- 加密组件信息 --- 加密组件你内容是从接收到客户端加密组件内筛选出来的  
> 	- Random_S: 随机数，用于生成后续对称加密交互过程要用的 session key  
![server hello](https://github.com/Saitoler/Network/blob/master/pics/server_hello%E6%95%B0%E6%8D%AE%E5%8C%85.png)  

### 步骤3  
服务器发送 Certificate 报文，报文中包含公开密钥证书  
如下图，网页中查看百度的证书信息， 可以看到根证书下还有两级证书链，在服务器发送 Certificate 报文中，这两个证书信息都包含在报文中。  

![certificate数据包](https://github.com/Saitoler/Network/blob/master/pics/certificate%E6%95%B0%E6%8D%AE%E5%8C%85.png)

### 步骤4  
服务器接着发送 Server Hello Done 报文，通知客户端第一阶段的 SSL 握手协商部分结束  
![server hello done 数据包](https://github.com/Saitoler/Network/blob/master/pics/server_hello_done%E6%95%B0%E6%8D%AE%E5%8C%85.png)。

### 步骤5
客户端发送 Client Key Exchange 报文，报文中包含通信加密中使用的一种称为 Pre-master secret 的随机密码串  
这个 pre-master secret 与前面 client 和 server 分别发送的 Random 随机数一起， 客户端与服务端会使用相同  
的算法，使用这三个随机数再生成一个随机数， 用于后续的对称密钥加密，即 session key.  

注意，这里报文中还带了 TLS 的版本号，之所以这里再带一次，是因为之前版本号是明文传输的，攻击者可能会恶意更改为较低  
的版本号，从而降低连接的安全系统方便其发起攻击。  

![client key exchange 数据包](https://github.com/Saitoler/Network/blob/master/pics/client_key_exchange%E6%95%B0%E6%8D%AE%E5%8C%85.png)

### 步骤6  
客户端继续发送 Change Cipher Spec 报文， 此报文会告知服务器，接下来的 Client  Finish 消息，将会用刚才协商的密钥  
Session key 来加密。  

### 步骤7  
客户端发送 Client Finished报文，此消息中，会列举客户端上面用到的所有加密字段，并计算出他们的 Hash 值，并用 Session key 加密处理。  

### 步骤8  
服务器向客户端发送 Change Cipher Spec 报文， 通知客户端接下来的消息会用 Session key 来加密  

### 步骤9
服务端发送 Server Finished 报文，同样的， 对服务器端整个协商阶段用到的所有字段算一个 Hash, 并用 Session key 加密  

## HTTPS 发展历程





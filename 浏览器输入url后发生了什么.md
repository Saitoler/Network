可以说是一道经典的面试题了
---

浏览器中输入 URL 后，到底发生了些什么？  
1. 浏览器确定协议类型：
> - 如果不输入协议类型，浏览器则默认使用 HTTP 协议  
> - 如果输入了协议类型，则以指定的协议类型为准，如直接输入了： https://www.163.com  
2. 解析域名： 浏览器从所输入的 URL 中解析出 **域名** 字段  
3. DNS解析：最终传输运送数据的是 TCP/IP 协议簇，这家快递公司只认IP地址才能发快递，因此要进行 DNS 解析。 
具体的详细过程如下：
> - 浏览器先查询浏览器缓存： 浏览器会缓存2-30min内的 DNS 查询结果  
> - 查询本地 HOST 文件： 浏览器缓存中没有该域名的 DNS记录时，会查询本地 HOST 文件中的记录  
> - 查询 DNS 服务器：  
	本地 HOST中没有时，则要向 TCP/IP 协议簇中所设置的 DNS服务器发送DNS查询请求
		*  DNS 封装好自己的查询请求后，给快递公司打电话，负责接洽 DNS 的是 UDP 协议这个收件人  
		*  UDP 再将数据扔给 IP 层  
		*  IP 协议查询路由表，很可能要出网关，这个网关不常见，要达到该网关需要其 MAC 地址  
		*  这时候 ARP 协议起作用，发起广播请求，获取到网关的 MAC 地址  
		*  IP 协议顺利到达网关，并将数据发送到 DNS 服务器
		*  DNS 服务器（本地设置的）收到请求，解析发现是 DNS 协议 53 端口，就知道这个是 DNS 查询请求了   
		> 先查询自己的本地缓存，本地缓存中有，就直接回复  
		> 若自己的本地缓存没有，则联系 DNS 域名系统的根服务器（. 全球一共13台）  
		> 根服务器若没有，就继续向下一级查询(com)， 直到获取到  
		*  获取到 DNS 结果后返回给浏览器
4. 既然已经拿到了 IP地址了，下一步就是建立 TCP 三次握手的过程了。  
5. 浏览器将 HTTP 请求封装好，由IP协议在上面建立的连接上发送出去，这个时候就分两种情况了：
> - 如果访问的是 http 类型网站，就直接交给 TCP 协议进行转发  
> - 如果访问的是 https 类型网站，就要先跟 ssl 协商好加密安全通道，再交给TCP协议进行转发  
6. 服务器收到浏览器的请求后，将所请求的页面返回，最终到达浏览器。  
7. TCP 四次挥手： 浏览器接收到返回请求后，进行 TCP 四次挥手，与服务器断开连接  
8. 浏览器解析所返回的文件，解析html 并进行页面渲染，最终得到整个网页  



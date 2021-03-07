##以下为极客时间的两日专题课程-《网络协议集训班》的内容总结 共15课时

重要程度星级：  
S:非常重要  
A:比较重要  
N:一般重要  
P:了解即可  

# 课时一 在浏览器上输入URL到主页显示，经历了哪些步骤(S)  
URL的基本介绍(协议、域名、端口、资源路径等等，以及域名的进一步介绍),可参考：  
https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/What_is_a_URL  
https://www.verisign.com/en_US/website-presence/online/what-is-a-url/index.xhtml   
![Image text](https://raw.githubusercontent.com/zwGithubStation/books/master/Network_Protocal_two_days/pic/what_is_URL.png)

**1.1 浏览器发起HTTP请求的逻辑**  
1.1.1 浏览器输入URL或者点击链接URL后，第一步是**域名解析为IP地址** 
关于域名系统(DNS)的原理，可参考：  
https://draveness.me/dns-coredns/  
关于DNS主要使用UDP的历史渊源，可参考：  
https://draveness.me/whys-the-design-dns-udp-tcp/  
当然各级大致有自己的缓存，比如浏览器本身就可以缓存了域名对应的IP地址  
该流程对应PPT中的下图部分：  
![Image text](https://raw.githubusercontent.com/zwGithubStation/books/master/Network_Protocal_two_days/pic/what_is_URL.png)  

1.1.2 http请求转化为传入IP地址的一个TCP连接  
通过IP地址，建立与服务的连接，服务可以是一个的负载均衡入口，其后台有一个水平扩展的集群(各个server的数据/代码/程序一致，提供同样的服务能力)，根据具体的请求类型，可以在server上直行下盘的数据库操作、内存上的读写缓存操作、亦或是kafka队列操作(mySQL/redis/kafka属于在TCP之上的协议)  
当然，图片、文件、css请求等也可能通过CDN访问用户就近的server，其连接server的过程同质于上述流程  
该流程对应PPT中的下图部分：  
![Image text](https://raw.githubusercontent.com/zwGithubStation/books/master/Network_Protocal_two_days/pic/what_is_URL.png)  

1.1.3 当然一个URL请求大概率不会只触发一个服务请求/建立一个TCP连接，这与http请求的DOM树划分对应，如果DOM树分解产生多个各类请求(html/js/css等)，则会触发多个B.1.2介绍的服务请求；最终各个请求结果在浏览器上组装呈现。  
http请求的DOM树划分、请求结果的合并呈现，不作为理解重点，了解即可。  
该流程对应PPT中的下图部分：   
![Image text](https://raw.githubusercontent.com/zwGithubStation/books/master/Network_Protocal_two_days/pic/what_is_URL.png)  

**1.2 OSI协议分层体系**  
1.2.1 OSI七层网络协议分层  
![Image text](https://raw.githubusercontent.com/zwGithubStation/books/master/Network_Protocal_two_days/pic/what_is_URL.png)   

1.2.2 重要的5层  
![Image text](https://raw.githubusercontent.com/zwGithubStation/books/master/Network_Protocal_two_days/pic/what_is_URL.png)    

1.2.3 负载均衡所在的层级 7层负载均衡、4层负载均衡的概念  
图如B.2.1所示  
四层负载均衡工作在UDP/TCP上，不会去解析上层的诸如tsl/ssl(不编译openssl库)、http协议的内容，只会建立两个TCP连接(负载均衡与客户端、负载均衡与服务器)或两个UDP session.  
LVS、iptables/ipvs可以工作在网络层或传输层，区别是网络层就只有一个TCP连接/UDP session(客户端与服务器)，而传输层就存在两个TCP连接/UDP session(负载均衡与客户端、负载均衡与服务器)，当然他们都不会解析上层的诸如tsl/ssl(不编译openssl库)、http协议的内容  
如图Nginx、HAProxy、Envoy可以实现四层负载均衡,也可以实现七层负载均衡。 工作在七层则可以解析tsl/ssl、http协议，将http协议直接解析为cgi(Common Gateway Interface,可以理解为http请求最终在server上所触发的动作，可以是python脚本，也可以是C/C++程序)，抑或是解析转换为redis操作。 e.g. python的uWSGI  

1.2.4 http、TSL/SSL、TCP、IP诸协议的工作流程图见课件PPT，不再赘述  
http:客户端发请求，服务端做响应; req/res都包含头+资源  
TLS/SSL:公开的消息通道中进行加密传输. c和s先协商出只有双方才知道的消息秘钥，后面传输的实际内容不解密无法看到  
TCP: 三次握手; 数据分割多消息传输,可靠传输  
IP: 路由器寻址最优路径, icmp等辅助实现传输   


**1.3 通过抓包缩小问题边界**  
抓包可以通过层层的以太网帧头部信息(以太网层的MAC头，IP层的IP头，TCP层的tcp头，HTTP层的header)剥离，获取信息    
关键路径：  
API Gateway、WAF防火墙等；  
各层对于包信息的修改：  
IP路由器：修改IP层信息的TTL等；  
TCP(四层)反向代理设备(e.g.F5):新建一条TCP连接(反向代理与server，新链上的包信息都有区别)  
HTTP(七层)反向代理设备：新的HTTP请求(反向代理与server，新请求可能是短连接，而client的http请求可能是keepalive长连接)   

** 课时综述：**   
1.OSI将协议分层后，可以让软件实现层面解耦合，降低了系统复杂性，允许各层协议独立演进。   
2.浏览器依据DOM树及渲染规则，会发出多个HTTP请求。  
3.DNS协议可以将域名转换为IP地址。  
4.OSI协议分层只是参考概念，许多实际的协议无法与其一一对应。  
5.IP协议保障了主机与主机间的报文可达。  
6.传输层协议提供了进程与进程间的报文可达。  
7.TCP协议提供了可靠的字节流传输。  
8.TLS/SSL协议提供了机密性、完整性及通讯双方的身份鉴别。  

# 课时二 如何抓包分析网络报文   
**2.1 HTTP/1协议的语法格式**  
HTTP的协议格式、基于ABNF严格描述的HTTP协议格式：  
该部分的基本内容不再详述，可在其他课程中再做详细了解(e.g. 透视HTTP协议课程)  
**2.2 浏览器的Network抓包面板**   
同上，需使用时再做细致了解  
**2.3 GUI中Wireshark抓包工具的用法**   
BPF表达式的使用不做细化了解。 本节同上，需使用时再做细致了解  
**2.4 shell中的TCPDump抓包工具的用法**   
本节同上，需使用时再做细致了解    

# 课时三 RESTful API该如何设计  
**3.1 Restful API的设计原则**   
  
**3.2 HTTP常见方法的含义**  
**3.3 HTTP常见响应码的含义**  
**3.4 HTTP常见头部的含义**  


# 课时四 哪些HTTP头部配合浏览器增强了Web安全

# 课时五 WAF防火墙究竟是怎样工作的？  

# 课时六 HTTP/1协议该如何优化性能？ 

# 课时七 升级到HTTP2协议有哪些好处？


# 课时八 下一代的HTTP3协议有哪些新特性？

# 课时九 TLS/SSL协议使用了哪些密码学知识？ 

# 课时九 TLS/SSL协议使用了哪些密码学知识？

# 课时十 如何优化TLS/SSL协议的性能、安全性？

# 课时十一 为何TCP连接要经由三次握手才能建立？

# 课时十二 如何优化TCP协议的性能？ 

# 课时十三 TCP的自动分包会引入哪些性能问题？

# 课时十四 为什么必须使用UDP协议发送广播报文？

# 课时十五 如何抓包分析K8S容器网络中的报文？ 
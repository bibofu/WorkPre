## 第一章：了解Web及网络基础

### 使用 HTTP 协议访问 Web

![2022-02-22-20-38-58.png](https://s2.loli.net/2022/02/22/hfrkPGgSc5Nbzna.png)



像这种通过发送请求获取服务器资源的 Web 浏览器等，都可称为客户端（client）

Web 使用一种名为 HTTP（HyperText Transfer Protocol，超文本传输协议 ）的协议作为规范，完成从客户端到服务器端等一系列运作流
程。而协议是指规则的约定。可以说，Web 是建立在 HTTP 协议上通信的



### HTTP 的诞生

#### 为知识共享而规划 Web

#### Web 成长时代

#### 驻足不前的 HTTP

==HTTP/0.9==
HTTP 于 1990 年问世。那时的 HTTP 并没有作为正式的标准被建立。现在的 HTTP 其实含有 HTTP1.0 之前版本的意思，因此被称为
HTTP/0.9。

==HTTP/1.0==
HTTP 正式作为标准被公布是在 1996 年的 5 月，版本被命名为HTTP/1.0，并记载于 RFC1945。虽说是初期标准，但该协议标准至今
仍被广泛使用在服务器端



==HTTP/1.1==

1997 年 1 月公布的 HTTP/1.1 是目前主流的 HTTP 协议版本。当初的标准是 RFC2068，之后发布的修订版 RFC2616 就是当前的最新版本。

可见，作为 Web 文档传输协议的 HTTP，它的版本几乎没有更新。新一代 HTTP/2.0 正在制订中，但要达到较高的使用覆盖率，仍需假以
时日。当年 HTTP 协议的出现主要是为了解决文本传输的难题。由于协议本身非常简单，于是在此基础上设想了很多应用方法并投入了实际使用。现在 HTTP 协议已经超出了 Web 这个框架的局限，被运用到了各种场景里



### 网络基础 TCP/IP

通常使用的网络（包括互联网）是在 TCP/IP 协议族的基础上运作的。而 HTTP 属于它内部的一个子集

#### TCP/IP 协议族

不同的硬件、操作系统之间的通信，所有的这一切都需要一种规则。而我们就把这种规则称为协议（protocol）

![2022-02-22-20-48-58.png](https://s2.loli.net/2022/02/22/hpAGxT1mcNEtHR7.png)

像这样把与互联网相关联的协议集合起来总称为 TCP/IP。也有说法认为，TCP/IP 是指 TCP 和 IP 这两种协议。还有一种说法认为，TCP/
IP 是在 IP 协议的通信过程中，使用到的协议族的统称。



#### TCP/IP 的分层管理

TCP/IP 协议族里重要的一点就是分层。TCP/IP 协议族按层次分别分为以下 4 层：==应用层、传输层、网络层和数据链路层==

TCP/IP 协议族各层的作用如下

==应用层==

应用层决定了向用户提供应用服务时通信的活动。TCP/IP 协议族内预存了各类通用的应用服务。比如，FTP（File
Transfer Protocol，文件传输协议）和 DNS（Domain Name System，域名系统）服务就是其中两类。HTTP 协议也处于该层

==传输层==

传输层对上层应用层，提供处于网络连接中的两台计算机之间的数据传输。
在传输层有两个性质不同的协议：TCP（Transmission ControlProtocol，传输控制协议）和 UDP（User Data Protocol，用户数据报
协议）。

==网络层==

网络层用来处理在网络上流动的数据包。数据包是网络传输的最小数据单位。该层规定了通过怎样的路径（所谓的传输路线）到达对方计算机，并把数据包传送给对方。与对方计算机之间通过多台计算机或网络设备进行传输时，网络层所起的作用就是在众多的选项内选择一条传输路线。

==链路层==

用来处理连接网络的硬件部分。包括控制操作系统、硬件的设备驱动、NIC（Network Interface Card，网络适配器，即网卡），及光纤等物理可见部分（还包括连接器等一切传输媒介）。硬件上的范畴均在链路层的作用范围之内

#### TCP/IP 通信传输流

![2022-02-22-20-55-48.png](https://s2.loli.net/2022/02/22/MRbEJQLHC5ycIGt.png)

利用 TCP/IP 协议族进行网络通信时，会通过分层顺序与对方进行通信。发送端从应用层往下走，接收端则往应用层往上走。我们用 HTTP 举例来说明，首先作为发送端的客户端在应用层（HTTP 协议）发出一个想看某个 Web 页面的 HTTP 请求。接着，为了传输方便，在传输层（TCP 协议）把从应用层处收到的数据（HTTP 请求报文）进行分割，并在各个报文上打上标记序号及端口号后转发给网络层。在网络层（IP 协议），增加作为通信目的地的 MAC 地址后转发给链路层。这样一来，发往网络的通信请求就准备齐全了。接收端的服务器在链路层接收到数据，按序往上层发送，一直到应用层。当传输到应用层，才能算真正接收到由客户端发送过来的 HTTP请求。

![2022-02-22-20-58-18.png](https://s2.loli.net/2022/02/22/8hSAuC5sFf4nmVk.png)



发送端在层与层之间传输数据时，每经过一层时必定会被打上一个该层所属的首部信息。反之，接收端在层与层传输数据时，每经过一层
时会把对应的首部消去。这种把数据信息包装起来的做法称为封装（encapsulate）



#### 与 HTTP 关系密切的协议 : IP、TCP 和DNS

##### 负责传输的 IP 协议

![2022-02-22-21-03-00.png](https://s2.loli.net/2022/02/22/FIBxaClRK7L5hof.png)



![](https://s3.bmp.ovh/imgs/2022/02/a197817aeb076faa.png)





##### 确保可靠性的 TCP 协议

按层次分，TCP 位于传输层，提供可靠的字节流服务。所谓的字节流服务（Byte Stream Service）是指，为了方便传输，将大块数据分割成以报文段（segment）为单位的数据包进行管理。而可靠的传输服务是指，能够把数据准确可靠地传给对方。一言以蔽之，TCP 协议为了更容易传送大数据才把数据分割，而且 TCP 协议能够确认数据最终是否送达到对方。

为了准确无误地将数据送达目标处，TCP 协议采用了三次握手（three-way handshaking）策略。用 TCP 协议把数据包送出去后，TCP不会对传送后的情况置之不理，它一定会向对方确认是否成功送达21握手过程中使用了 TCP 的标志（flag） —— SYN（synchronize） 和ACK（acknowledgement）。

![](https://s3.bmp.ovh/imgs/2022/02/c91f721699b122f7.png)



#### 负责域名解析的 DNS 服务

DNS（Domain Name System）服务是和 HTTP 协议一样位于应用层的协议。它提供域名到 IP 地址之间的解析服务

DNS 协议提供通过域名查找 IP 地址，或逆向从 IP 地址反查域名的服务

![](https://s3.bmp.ovh/imgs/2022/02/e37e68d4acf11da7.png)



#### 各种协议与 HTTP 协议的关系

学习了和 HTTP 协议密不可分的 TCP/IP 协议族中的各种协议后，我们再通过这张图来了解下 IP 协议、TCP 协议和 DNS 服务在使用HTTP 协议的通信过程中各自发挥了哪些作用。

![](https://s3.bmp.ovh/imgs/2022/02/4fb9122de39fc1ff.png)

#### URI 和 URL

与 URI（统一资源标识符）相比，我们更熟悉 URL（UniformResource Locator，统一资源定位符）。URL正是使用 Web 浏览器等
访问 Web 页面时需要输入的网页地址。比如，下图的 http://hackr.jp/就是 URL

##### 统一资源标识符

URI 是 Uniform Resource Identifier 的缩写。RFC2396 分别对这 3 个单词进行了如下定义。

==Uniform==

规定统一的格式可方便处理多种不同类型的资源，而不用根据上下文环境来识别资源指定的访问方式。另外，加入新增的协议方案（如
http: 或 ftp:）也更容易

==Resource==

资源的定义是“可标识的任何东西”。除了文档文件、图像或服务（例如当天的天气预报）等能够区别于其他类型的，全都可作为资源。另外，资源不仅可以是单一的，也可以是多数的集合体

==Identifier==

表示可标识的对象。也称为标识符。综上所述，URI 就是由某个协议方案表示的资源的定位标识符。协议方案是指访问资源所使用的协议类型名称

![](https://s3.bmp.ovh/imgs/2022/02/7607198aa8f470ef.png)



##### URI 格式

表示指定的 URI，要使用涵盖全部必要信息的绝对 URI、绝对 URL以及相对 URL。相对 URL，是指从浏览器中基本 URI 处指定的 URL，
形如 /image/logo.gif

==绝对 URI 的格式==

![](https://s3.bmp.ovh/imgs/2022/02/b50919bb7455bfe7.png)





## 第二章：简单的 HTTP 协议

本章将针对 HTTP 协议结构进行讲解，主要使用HTTP/1.1版本。

### HTTP 协议用于客户端和服务器端之间的通信

![](https://s3.bmp.ovh/imgs/2022/02/7f3d95b9675817c2.png)

有时候，按实际情况，两台计算机作为客户端和服务器端的角色有可能会互换。但就仅从一条通信路线来说，服务器端和客户端的角色是
确定的，而用 HTTP 协议能够明确区分哪端是客户端，哪端是服务器端

### 通过请求和响应的交换达成通信

![](https://s3.bmp.ovh/imgs/2022/02/e5286c57f9e396e1.png)

HTTP 协议规定，请求从客户端发出，最后服务器端响应该请求并返回。换句话说，肯定是先从客户端开始建立通信的，服务器端在没有接收到请求之前不会发送响应。

![](https://s3.bmp.ovh/imgs/2022/02/960b75f5c084d71e.png)

起始行开头的GET表示请求访问服务器的类型，称为方法（method）。随后的字符串 /index.htm 指明了请求访问的资源对象，也叫做请求 URI（request-URI）。最后的 HTTP/1.1，即 HTTP 的版本号，用来提示客户端使用的 HTTP 协议功能。

综合来看，这段请求内容的意思是：请求访问某台 HTTP 服务器上的/index.htm 页面资源。请求报文是由请求方法、请求 URI、协议版本、可选的请求首部字段和内容实体构成的

[![bpikS1.png](https://s4.ax1x.com/2022/02/22/bpikS1.png)](https://imgtu.com/i/bpikS1)

响应报文：

[![bpi86P.png](https://s4.ax1x.com/2022/02/22/bpi86P.png)](https://imgtu.com/i/bpi86P)



[![bpir60.png](https://s4.ax1x.com/2022/02/22/bpir60.png)](https://imgtu.com/i/bpir60)



### HTTP 是不保存状态的协议

HTTP 是一种不保存状态，即无状态（stateless）协议。HTTP 协议自身不对请求和响应之间的通信状态进行保存。也就是说在 HTTP 这个级别，协议对于发送过的请求或响应都不做持久化处理

[![bpiI6x.png](https://s4.ax1x.com/2022/02/22/bpiI6x.png)](https://imgtu.com/i/bpiI6x)



使用 HTTP 协议，每当有新的请求发送时，就会有对应的新响应产生。协议本身并不保留之前一切的请求或响应报文的信息。这是为了
更快地处理大量事务，确保协议的可伸缩性，而特意把 HTTP 协议设计成如此简单的

可是，随着 Web 的不断发展，因无状态而导致业务处理变得棘手的情况增多了。比如，用户登录到一家购物网站，即使他跳转到该站的
其他页面后，也需要能继续保持登录状态。针对这个实例，网站为了能够掌握是谁送出的请求，需要保存用户的状态。

HTTP/1.1 虽然是无状态协议，但为了实现期望的保持状态功能，于是引入了==Cookie 技术==。有了 Cookie 再用 HTTP 协议通信，就可以管
理状态了。

### 请求 URI 定位资源

HTTP 协议使用 URI 定位互联网上的资源。正是因为 URI 的特定功能，在互联网上任意位置的资源都能访问到。

[![bpF0ED.png](https://s4.ax1x.com/2022/02/22/bpF0ED.png)](https://imgtu.com/i/bpF0ED)

当客户端请求访问资源而发送请求时，URI 需要将作为请求报文中的请求 URI 包含在内。指定请求 URI 的方式有很多

[![bpFObT.png](https://s4.ax1x.com/2022/02/22/bpFObT.png)](https://imgtu.com/i/bpFObT)



### 告知服务器意图的 HTTP 方法

下面，我们介绍 HTTP/1.1 中可使用的方法。

==GET ：获取资源==

[![bpkQMt.png](https://s4.ax1x.com/2022/02/22/bpkQMt.png)](https://imgtu.com/i/bpkQMt)

[![bpk1qf.png](https://s4.ax1x.com/2022/02/22/bpk1qf.png)](https://imgtu.com/i/bpk1qf)



==POST：传输实体主体==

[![bpksdU.png](https://s4.ax1x.com/2022/02/22/bpksdU.png)](https://imgtu.com/i/bpksdU)



==PUT：传输文件==

PUT 方法用来传输文件。就像 FTP 协议的文件上传一样，要求在请求报文的主体中包含文件内容，然后保存到请求 URI 指定的位置。

但是，鉴于 HTTP/1.1 的 PUT 方法自身不带验证机制，任何人都可以上传文件 , 存在安全性问题，因此一般的 Web 网站不使用该方法。若配合 Web 应用程序的验证机制，或架构设计采用REST（REpresentational State Transfer，表征状态转移）标准的同类Web 网站，就可能会开放使用 PUT 方法

[![bpkOSA.png](https://s4.ax1x.com/2022/02/22/bpkOSA.png)](https://imgtu.com/i/bpkOSA)



==HEAD：获得报文首部==

HEAD 方法和 GET 方法一样，只是不返回报文主体部分。用于确认URI 的有效性及资源更新的日期时间等

![](https://s3.bmp.ovh/imgs/2022/02/cc11079701719355.png)

![](https://s3.bmp.ovh/imgs/2022/02/5b8cfe884769b294.png)



==DELETE：删除文件==

DELETE 方法用来删除文件，是与 PUT 相反的方法。DELETE 方法按请求 URI 删除指定的资源。
但是，HTTP/1.1 的 DELETE 方法本身和 PUT 方法一样不带验证机制，所以一般的 Web 网站也不使用 DELETE 方法。当配合 Web 应用
程序的验证机制，或遵守 REST 标准时还是有可能会开放使用的。

![](https://s3.bmp.ovh/imgs/2022/02/58f2723b1972a0e7.png)



==OPTIONS：询问支持的方法==

OPTIONS 方法用来查询针对请求 URI 指定的资源支持的方法。

![](https://s3.bmp.ovh/imgs/2022/02/a0f07b63f5a36380.png)



==TRACE：追踪路径==

TRACE 方法是让 Web 服务器端将之前的请求通信环回给客户端的方法

客户端通过 TRACE 方法可以查询发送出去的请求是怎样被加工修改/ 篡改的。这是因为，请求想要连接到源目标服务器可能会通过代理中转，TRACE 方法就是用来确认连接过程中发生的一系列操作

![](https://s3.bmp.ovh/imgs/2022/02/7d8dd8c38cb04137.png)



==CONNECT：要求用隧道协议连接代理==

[![b9ZgdP.png](https://s4.ax1x.com/2022/02/23/b9ZgdP.png)](https://imgtu.com/i/b9ZgdP)



### 使用方法下达命令

向请求 URI 指定的资源发送请求报文时，采用称为方法的命令。方法的作用在于，可以指定请求的资源按期望产生某种行为。方法中有 GET、POST 和 HEAD 等。

[![b9Zbd0.png](https://s4.ax1x.com/2022/02/23/b9Zbd0.png)](https://imgtu.com/i/b9Zbd0)



[![b9ZOiT.png](https://s4.ax1x.com/2022/02/23/b9ZOiT.png)](https://imgtu.com/i/b9ZOiT)



### 持久连接节省通信量

HTTP 协议的初始版本中，每进行一次 HTTP 通信就要断开一次 TCP连接。

[![b9eZSe.png](https://s4.ax1x.com/2022/02/23/b9eZSe.png)](https://imgtu.com/i/b9eZSe)

[![b9eMwt.png](https://s4.ax1x.com/2022/02/23/b9eMwt.png)](https://imgtu.com/i/b9eMwt)

#### 持久连接

为解决上述 TCP 连接的问题，HTTP/1.1 和一部分的 HTTP/1.0 想出了持久连接（HTTP Persistent Connections，也称为 HTTP keep-alive 或HTTP connection reuse）的方法。持久连接的特点是，只要任意一端没有明确提出断开连接，则保持 TCP 连接状态

[![b9eamn.png](https://s4.ax1x.com/2022/02/23/b9eamn.png)](https://imgtu.com/i/b9eamn)

持久连接的好处在于减少了 TCP 连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载。另外，减少开销的那部分时间，使HTTP 请求和响应能够更早地结束，这样 Web 页面的显示速度也就相应提高了。在 HTTP/1.1 中，所有的连接默认都是持久连接，但在 HTTP/1.0 内并未标准化。虽然有一部分服务器通过非标准的手段实现了持久连接，但服务器端不一定能够支持持久连接。毫无疑问，除了服务器端，客户端也需要支持持久连接。



#### 管线化

持久连接使得多数请求以管线化（pipelining）方式发送成为可能。从前发送请求后需等待并收到响应，才能发送下一个请求。管线化技术出现后，不用等待响应亦可直接发送下一个请求。这样就能够做到同时并行发送多个请求，而不需要一个接一个地等待响应了。

[![b9e5tK.png](https://s4.ax1x.com/2022/02/23/b9e5tK.png)](https://imgtu.com/i/b9e5tK)



### 使用 Cookie 的状态管理

[![b9mPXj.png](https://s4.ax1x.com/2022/02/23/b9mPXj.png)](https://imgtu.com/i/b9mPXj)

保留无状态协议这个特征的同时又要解决类似的矛盾问题，于是引入了 Cookie 技术。Cookie 技术通过在请求和响应报文中写入 Cookie 信息来控制客户端的状态

Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie 的首部字段信息，通知客户端保存 Cookie。当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入 Cookie 值后发送出去。服务器端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息

[![b9nmVI.png](https://s4.ax1x.com/2022/02/23/b9nmVI.png)](https://imgtu.com/i/b9nmVI)

[![b9nMPf.png](https://s4.ax1x.com/2022/02/23/b9nMPf.png)](https://imgtu.com/i/b9nMPf)



HTTP 请求报文和响应报文的内容如下：

[![b9ngd1.png](https://s4.ax1x.com/2022/02/23/b9ngd1.png)](https://imgtu.com/i/b9ngd1)



## 第三章：HTTP 报文内的 HTTP信息

### HTTP 报文

用于 HTTP 协议交互的信息被称为 HTTP 报文。请求端（客户端）的HTTP 报文叫做请求报文，响应端（服务器端）的叫做响应报文。
HTTP 报文本身是由多行（用 CR+LF 作换行符）数据构成的字符串文本。
HTTP 报文大致可分为报文首部和报文主体两块。两者由最初出现的空行（CR+LF）来划分。通常，并不一定要有报文主体

[![b9uzh6.png](https://s4.ax1x.com/2022/02/23/b9uzh6.png)](https://imgtu.com/i/b9uzh6)



#### 请求报文及响应报文的结构

[![b9Ku38.png](https://s4.ax1x.com/2022/02/23/b9Ku38.png)](https://imgtu.com/i/b9Ku38)



[![b9KJNq.png](https://s4.ax1x.com/2022/02/23/b9KJNq.png)](https://imgtu.com/i/b9KJNq)



请求报文和响应报文的首部内容由以下数据组成。现在出现的各种首部字段及状态码稍后会进行阐述。
==请求行==
包含用于请求的方法，请求 URI 和 HTTP 版本。
==状态行==
包含表明响应结果的状态码，原因短语和 HTTP 版本。
==首部字段==
包含表示请求和响应的各种条件和属性的各类首部。一般有 4 种首部，分别是：通用首部、请求首部、响应首部和实体首部。
==其他==
可能包含 HTTP 的 RFC 里未定义的首部（Cookie 等）

### 编码提升传输速率


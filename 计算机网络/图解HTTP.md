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



### 请求报文及响应报文的结构

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

HTTP 在传输数据时可以按照数据原貌直接传输，但也可以在传输过程中通过编码提升传输速率。通过在传输时编码，能有效地处理大量
的访问请求。但是，编码的操作需要计算机来完成，因此会消耗更多的 CPU 等资源

#### 报文主体和实体主体的差异

[![b94dzR.png](https://s4.ax1x.com/2022/02/23/b94dzR.png)](https://imgtu.com/i/b94dzR)



#### 压缩传输的内容编码

向待发送邮件内增加附件时，为了使邮件容量变小，我们会先用 ZIP压缩文件之后再添加附件发送。HTTP 协议中有一种被称为内容编码
的功能也能进行类似的操作。内容编码指明应用在实体内容上的编码格式，并保持实体信息原样压缩。内容编码后的实体由客户端接收并负责解码

[![b95dAS.png](https://s4.ax1x.com/2022/02/23/b95dAS.png)](https://imgtu.com/i/b95dAS)



#### 分割发送的分块传输编码

在 HTTP 通信过程中，请求的编码实体资源尚未全部传输完成之前，浏览器无法显示请求页面。在传输大容量数据时，通过把数据分割成多块，能够让浏览器逐步显示页面。这种把实体主体分块的功能称为分块传输编码（Chunked TransferCoding）。

[![b9ICut.png](https://s4.ax1x.com/2022/02/23/b9ICut.png)](https://imgtu.com/i/b9ICut)



### 发送多种数据的多部分对象集合

[![b9o1sI.png](https://s4.ax1x.com/2022/02/23/b9o1sI.png)](https://imgtu.com/i/b9o1sI)

发送邮件时，我们可以在邮件里写入文字并添加多份附件。这是因为采用了 MIME（Multipurpose Internet Mail Extensions，多用途因特网邮件扩展）机制，它允许邮件处理文本、图片、视频等多个不同类型的数据。例如，图片等二进制数据以 ASCII 码字符串编码的方式指明，就是利用 MIME 来描述标记数据类型。而在 MIME 扩展中会使用一种称为多部分对象集合（Multipart）的方法，来容纳多份不同类型的数据。





### 获取部分内容的范围请求

以前，用户不能使用现在这种高速的带宽访问互联网，当时，下载个尺寸稍大的图片或文件就已经很吃力了。如果下载过程中遇到网络中断的情况，那就必须重头开始。为了解决上述问题，需要一种可恢复的机制。所谓恢复是指能从之前下载中断处恢复下载。要实现该功能需要指定下载的实体范围。像这样，指定范围发送的请求叫做范围请求（Range Request）



### 内容协商返回最合适的内容

同一个 Web 网站有可能存在着多份相同内容的页面。比如英语版和中文版的 Web 页面，它们内容上虽相同，但使用的语言却不同。
当浏览器的默认语言为英语或中文，访问相同 URI 的 Web 页面时，则会显示对应的英语版或中文版的 Web 页面。这样的机制称为内容
协商（Content Negotiation）



## 第四章：返回结果的 HTTP 状态码

HTTP 状态码负责表示客户端 HTTP 请求的返回结果、标记服务器端的处理是否正常、通知出现的错误等工作。

### 状态码告知从服务器端返回的请求结果

状态码的职责是当客户端向服务器端发送请求时，描述返回的请求结果。借助状态码，用户可以知道服务器端是正常处理了请求，还是出现了错误。

[![b9T3p4.png](https://s4.ax1x.com/2022/02/23/b9T3p4.png)](https://imgtu.com/i/b9T3p4)

状态码如 200 OK，以 3 位数字和原因短语组成。
数字中的第一位指定了响应类别，后两位无分类。响应类别有以下 5种

[![b9Tscd.png](https://s4.ax1x.com/2022/02/23/b9Tscd.png)](https://imgtu.com/i/b9Tscd)



### 2XX 成功

#### 200 OK

[![b97CuR.png](https://s4.ax1x.com/2022/02/23/b97CuR.png)](https://imgtu.com/i/b97CuR)



在响应报文内，随状态码一起返回的信息会因方法的不同而发生改变。比如，使用 GET 方法时，对应请求资源的实体会作为响应返回；而使用 HEAD 方法时，对应请求资源的实体首部不随报文主体作为响应返回（即在响应中只返回首部，不会返回实体的主体部分）。

#### 204 No Content

[![b976GF.png](https://s4.ax1x.com/2022/02/23/b976GF.png)](https://imgtu.com/i/b976GF)

该状态码代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外，也不允许返回任何实体的主体。比如，
当从浏览器发出请求处理后，返回 204 响应，那么浏览器显示的页面不发生更新。
一般在只需要从客户端往服务器发送信息，而对客户端不需要发送新信息内容的情况下使用

#### 206 Partial Content

[![b97xdP.png](https://s4.ax1x.com/2022/02/23/b97xdP.png)](https://imgtu.com/i/b97xdP)

该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET 请求。响应报文中包含由 Content-Range 指定范围的实体内容



### 3XX 重定向

3XX 响应结果表明浏览器需要执行某些特殊的处理以正确处理请求。

#### 301 Moved Permanently

[![b9HNFK.png](https://s4.ax1x.com/2022/02/23/b9HNFK.png)](https://imgtu.com/i/b9HNFK)

永久性重定向。该状态码表示请求的资源已被分配了新的 URI，以后应使用资源现在所指的 URI。也就是说，如果已经把资源对应的 URI
保存为书签了，这时应该按 Location 首部字段提示的 URI 重新保存

#### 302 Found

[![b9bPk6.png](https://s4.ax1x.com/2022/02/23/b9bPk6.png)](https://imgtu.com/i/b9bPk6)

和 301 Moved Permanently 状态码相似，但 302 状态码代表的资源不是被永久移动，只是临时性质的。换句话说，已移动的资源对应的
URI 将来还有可能发生改变。



#### 303 See Other

[![b9b0hT.png](https://s4.ax1x.com/2022/02/23/b9b0hT.png)](https://imgtu.com/i/b9b0hT)

该状态码表示由于请求对应的资源存在着另一个 URI，应使用 GET方法定向获取请求的资源。
303 状态码和 302 Found 状态码有着相同的功能，但 303 状态码明确表示客户端应当采用 GET 方法获取资源，这点与 302 状态码有区
别

>   当 301、302、303 响应状态码返回时，几乎所有的浏览器都会把POST 改成 GET，并删除请求报文内的主体，之后请求会自动再次发送。301、302 标准是禁止将 POST 方法改变成 GET 方法的，但实际使用时大家都会这么做。

#### 304 Not Modified

[![b9boge.png](https://s4.ax1x.com/2022/02/23/b9boge.png)](https://imgtu.com/i/b9boge)

该状态码表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但未满足条件的情况。304 状态码返回时，不包含任何响应
的主体部分。304 虽然被划分在 3XX 类别中，但是==和重定向没有关系==。

附带条件的请求是指采用 GET方法的请求报文中包含 If-Match，If-ModifiedSince，If-None-Match，If-Range，If-Unmodified-Since 中任一首部

#### 307 Temporary Redirect

临时重定向。该状态码与 302 Found 有着相同的含义。



### 4XX 客户端错误

4XX 的响应结果表明客户端是发生错误的原因所在

#### 400 Bad Request

[![b9LZdI.png](https://s4.ax1x.com/2022/02/23/b9LZdI.png)](https://imgtu.com/i/b9LZdI)

该状态码表示请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求

#### 401 Unauthorized

[![b9L6T1.png](https://s4.ax1x.com/2022/02/23/b9L6T1.png)](https://imgtu.com/i/b9L6T1)

该状态码表示发送的请求需要有通过 HTTP 认证（BASIC 认证、DIGEST 认证）的认证信息。另外若之前已进行过 1 次请求，则表示用 户认证失败。

#### 403 Forbidden

[![b9Op0s.png](https://s4.ax1x.com/2022/02/23/b9Op0s.png)](https://imgtu.com/i/b9Op0s)



未获得文件系统的访问授权，访问权限出现某些问题（从未授权的发送源 IP 地址试图访问）等列举的情况都可能是发生 403 的原因。

#### 404 Not Found

[![b9OMA1.png](https://s4.ax1x.com/2022/02/23/b9OMA1.png)](https://imgtu.com/i/b9OMA1)

该状态码表明服务器上无法找到请求的资源



### 5XX 服务器错误

5XX 的响应结果表明服务器本身发生错误。

#### 500 Internal Server Error

[![b9OD9f.png](https://s4.ax1x.com/2022/02/23/b9OD9f.png)](https://imgtu.com/i/b9OD9f)

该状态码表明服务器端在执行请求时发生了错误。也有可能是 Web应用存在的 bug 或某些临时的故障

#### 503 Service Unavailable

[![b9OogU.png](https://s4.ax1x.com/2022/02/23/b9OogU.png)](https://imgtu.com/i/b9OogU)

该状态码表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求



### 总结

| 状态码                    | 描述                           |
| ------------------------- | ------------------------------ |
| 200 OK                    | 正常处理                       |
| 204 No Content            | 正常处理，返回报文不含实体主题 |
| 206 Partial Content       | 正常处理，返回部分实体         |
| 301 Moved Permanently     | 永久移动                       |
| 302 Found                 | 暂时移动                       |
| 303 See Other             | 暂时移动，应使用GET            |
| 304 Not Modified          | 客户端附带条件，条件不满足     |
| 307 Temporary             | 同302                          |
| 400 Bad Request           | 请求报文有问题                 |
| 401 Unauthorized          | 未认证                         |
| 403 Forbidden             | 被拒绝，可能因为源IP等         |
| 404 Not Found             | 资源没找到                     |
| 500 Internal Server Error | 服务端内部错误                 |
| 503 Service Unavailable   | 服务访问量大，暂时不可用       |



## 第五章：与 HTTP 协作的 Web 服务器

一台 Web 服务器可搭建多个独立域名的 Web 网站，也可作为通信路径上的中转服务器提升传输效率。

### 用单台虚拟主机实现多个域名

HTTP/1.1 规范允许一台 HTTP 服务器搭建多个 Web 站点。比如，提供 Web 托管服务（Web Hosting Service）的供应商，可以用一台服务器为多位客户服务，也可以以每位客户持有的域名运行各自不同的网站。这是因为利用了虚拟主机（Virtual Host，又称虚拟服务器）的功能。即使物理层面只有一台服务器，但只要使用虚拟主机的功能，则可以假想已具有多台服务器。

[![b9xe8x.png](https://s4.ax1x.com/2022/02/23/b9xe8x.png)](https://imgtu.com/i/b9xe8x)

[![b9xaM8.png](https://s4.ax1x.com/2022/02/23/b9xaM8.png)](https://imgtu.com/i/b9xaM8)

在相同的 IP 地址下，由于虚拟主机可以寄存多个不同主机名和域名的 Web 网站，因此在发送 HTTP 请求时，必须在 Host 首部内完整指定主机名或域名的 URI。

### 通信数据转发程序 ：代理、网关、隧道

HTTP 通信时，除客户端和服务器以外，还有一些用于通信数据转发的应用程序，例如代理、网关和隧道。它们可以配合服务器工作。

#### 代理

代理是一种有转发功能的应用程序，它扮演了位于服务器和客户端“中间人”的角色，接收由客户端发送的请求并转发给服务器，同时也接收服务器返回的响应并转发给客户端。

代理服务器的基本行为就是接收客户端发送的请求后转发给其他服务器。代理不改变请求 URI，会直接发送给前方持有资源的目标服务
器。持有资源实体的服务器被称为源服务器。从源服务器返回的响应经过代理服务器后再传给客户端

[![b9zeoj.png](https://s4.ax1x.com/2022/02/23/b9zeoj.png)](https://imgtu.com/i/b9zeoj)



使用代理服务器的理由有：利用缓存技术（稍后讲解）减少网络带宽的流量，组织内部针对特定网站的访问控制，以获取访问日志为主要
目的，等等。代理有多种使用方法，按两种基准分类。一种是是否使用缓存，另一种是是否会修改报文。
==缓存代理==
代理转发响应时，缓存代理（Caching Proxy）会预先将资源的副本（缓存）保存在代理服务器上。当代理再次接收到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回。
==透明代理==
转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理（Transparent Proxy）。反之，对报文内容进行加工的代理被称为非透明代理。



#### 网关

网关是转发其他服务器通信数据的服务器，接收从客户端发送来的请求时，它就像自己拥有资源的源服务器一样对请求进行处理。有时客户端可能都不会察觉，自己的通信目标是一个网关。

[![b9zxXT.png](https://s4.ax1x.com/2022/02/23/b9zxXT.png)](https://imgtu.com/i/b9zxXT)

网关的工作机制和代理十分相似。而网关能使通信线路上的服务器提供非 HTTP 协议服务。利用网关能提高通信的安全性，因为可以在客户端与网关之间的通信线路上加密以确保连接的安全。比如，网关可以连接数据库，使用SQL语句查询数据。另外，在 Web 购物网站上进行信用卡结算时，网关可以和信用卡结算系统联动。

#### 隧道

隧道是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序。

隧道可按要求建立起一条与其他服务器的通信线路，届时使用 SSL等加密手段进行通信。隧道的目的是确保客户端能与服务器进行安全的通信。隧道本身不会去解析 HTTP 请求。也就是说，请求保持原样中转给之后的服务器。隧道会在通信双方断开连接时结束。

[![bCSuHe.png](https://s4.ax1x.com/2022/02/23/bCSuHe.png)](https://imgtu.com/i/bCSuHe)





### 保存资源的缓存

缓存是指代理服务器或客户端本地磁盘内保存的资源副本。利用缓存可减少对源服务器的访问，因此也就节省了通信流量和通信时间。
缓存服务器是代理服务器的一种，并归类在缓存代理类型中。换句话说，当代理转发从服务器返回的响应时，代理服务器将会保存一份资
源的副本

[![bCSUHg.png](https://s4.ax1x.com/2022/02/23/bCSUHg.png)](https://imgtu.com/i/bCSUHg)

[![bCScuT.png](https://s4.ax1x.com/2022/02/23/bCScuT.png)](https://imgtu.com/i/bCScuT)

缓存服务器的优势在于利用缓存可避免多次从源服务器转发资源。因此客户端可就近从缓存服务器上获取资源，而源服务器也不必多次处理相同的请求了。

#### 缓存的有效期限

[![bCSI81.png](https://s4.ax1x.com/2022/02/23/bCSI81.png)](https://imgtu.com/i/bCSI81)

#### 客户端的缓存

缓存不仅可以存在于缓存服务器内，还可以存在客户端浏览器中。以Internet Explorer 程序为例，把客户端缓存称为临时网络文件
（Temporary Internet File）。
浏览器缓存如果有效，就不必再向服务器请求相同的资源了，可以直接从本地磁盘内读取。另外，和缓存服务器相同的一点是，当判定缓存过期后，会向源服务器确认资源的有效性。若判断浏览器缓存失效，浏览器会再次请求新资源

[![bCSXUH.png](https://s4.ax1x.com/2022/02/23/bCSXUH.png)](https://imgtu.com/i/bCSXUH)





## 第六章：HTTP 首部

HTTP 协议的请求和响应报文中必定包含 HTTP 首部，只是我们平时在使用 Web 的过程中感受不到它

### HTTP 报文首部

[![bCC83R.png](https://s4.ax1x.com/2022/02/23/bCC83R.png)](https://imgtu.com/i/bCC83R)

HTTP 协议的请求和响应报文中必定包含 HTTP 首部。首部内容为客户端和服务器分别处理请求和响应提供所需要的信息。对于客户端用
户来说，这些信息中的大部分内容都无须亲自查看。报文首部由几个字段构成。

[![bCCI8s.png](https://s4.ax1x.com/2022/02/23/bCCI8s.png)](https://imgtu.com/i/bCCI8s)

在报文众多的字段当中，HTTP 首部字段包含的信息最为丰富。首部字段同时存在于请求和响应报文内，并涵盖 HTTP 报文相关的内容信
息。因 HTTP 版本或扩展规范的变化，首部字段可支持的字段内容略有不同。本书主要涉及 HTTP/1.1 及常用的首部字段



### HTTP 首部字段

#### HTTP 首部字段传递重要信息

HTTP 首部字段是构成 HTTP 报文的要素之一。在客户端与服务器之间以 HTTP 协议进行通信的过程中，无论是请求还是响应都会使用首
部字段，它能起到传递额外重要信息的作用。使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容

![](https://s3.bmp.ovh/imgs/2022/02/76a3af88fe8a936a.png)



#### HTTP 首部字段结构

![](https://s3.bmp.ovh/imgs/2022/02/5731bea92b109bef.png)



>若 HTTP 首部字段重复了会如何
>
>当 HTTP 报文首部中出现了两个或两个以上具有相同首部字段名时会怎么样？这种情况在规范内尚未明确，根据浏览器内部处理逻辑的不同，结果可能并不一致。有些浏览器会优先处理第一次出现的首部字段，而有些则会优先处理最后出现的首部字段

#### 4 种 HTTP 首部字段类型

![](https://s3.bmp.ovh/imgs/2022/02/c92d0b4d655c1f75.png)



#### HTTP/1.1 首部字段一览

==通用首部字段==

![](https://s3.bmp.ovh/imgs/2022/02/527912ee9e44f82b.png)

==请求首部字段==

![](https://s3.bmp.ovh/imgs/2022/02/d5647667086a1979.png)

==响应首部字段==

[![bC4U9H.png](https://s4.ax1x.com/2022/02/23/bC4U9H.png)](https://imgtu.com/i/bC4U9H)



==实体首部字段==

[![bC4But.png](https://s4.ax1x.com/2022/02/23/bC4But.png)](https://imgtu.com/i/bC4But)



#### 非 HTTP/1.1 首部字段

在 HTTP 协议通信交互中使用到的首部字段，不限于 RFC2616 中定义的 47 种首部字段。还有 Cookie、Set-Cookie 和 Content-Disposition等在其他 RFC 中定义的首部字段，它们的使用频率也很高



#### End-to-end 首部和 Hop-by-hop 首部

### HTTP/1.1 通用首部字段

通用首部字段是指，请求报文和响应报文双方都会使用的首部。

#### Cache-Control

通过指定首部字段 Cache-Control 的指令，就能操作缓存的工作机制

[![bC57Lt.png](https://s4.ax1x.com/2022/02/23/bC57Lt.png)](https://imgtu.com/i/bC57Lt)

#### Connection

Connection 首部字段具备如下两个作用。

1.  控制不再转发给代理的首部字段
2.  管理持久连接

#### Date

[![bCIztO.png](https://s4.ax1x.com/2022/02/23/bCIztO.png)](https://imgtu.com/i/bCIztO)

>   Date: Tue, 03 Jul 2012 04:40:59 GMT

#### Pragma

[![bCoMcj.png](https://s4.ax1x.com/2022/02/23/bCoMcj.png)](https://imgtu.com/i/bCoMcj)

#### Trailer

[![bCo5DI.png](https://s4.ax1x.com/2022/02/23/bCo5DI.png)](https://imgtu.com/i/bCo5DI)



#### Transfer-Encoding

[![bCoxrn.png](https://s4.ax1x.com/2022/02/23/bCoxrn.png)](https://imgtu.com/i/bCoxrn)

首部字段 Transfer-Encoding 规定了传输报文主体时采用的编码方式。



#### Upgrade

首部字段 Upgrade 用于检测 HTTP 协议及其他协议是否可使用更高的版本进行通信，其参数值可以用来指定一个完全不同的通信协议

[![bCTNZt.png](https://s4.ax1x.com/2022/02/23/bCTNZt.png)](https://imgtu.com/i/bCTNZt)



####  Via

使用首部字段 Via 是为了追踪客户端与服务器之间的请求和响应报文的传输路径。报文经过代理或网关时，会先在首部字段 Via 中附加该服务器的信息，然后再进行转发。这个做法和 traceroute 及电子邮件的 Received首部的工作机制很类

[![bCT4WF.png](https://s4.ax1x.com/2022/02/23/bCT4WF.png)](https://imgtu.com/i/bCT4WF)



#### Warning

HTTP/1.1 的 Warning 首部是从 HTTP/1.0 的响应首部（Retry-After）演变过来的。该首部通常会告知用户一些与缓存相关的问题的警告。



### 请求首部字段

请求首部字段是从客户端往服务器端发送请求报文中所使用的字段，用于补充请求的附加信息、客户端信息、对响应内容相关的优先级等
内容

[![bCTLo6.png](https://s4.ax1x.com/2022/02/23/bCTLo6.png)](https://imgtu.com/i/bCTLo6)



#### Accept

[![bC7pyd.png](https://s4.ax1x.com/2022/02/23/bC7pyd.png)](https://imgtu.com/i/bC7pyd)

Accept 首部字段可通知服务器，用户代理能够处理的媒体类型及媒体类型的相对优先级。可使用 type/subtype 这种形式，一次指定多种媒体类型



#### Accept-Charset

[![bC7xA0.png](https://s4.ax1x.com/2022/02/23/bC7xA0.png)](https://imgtu.com/i/bC7xA0)

Accept-Charset 首部字段可用来通知服务器用户代理支持的字符集及字符集的相对优先顺序。另外，可一次性指定多种字符集。与首部字
段 Accept 相同的是可用权重 q 值来表示相对优先级。

#### Accept-Encoding

[![bC78YT.png](https://s4.ax1x.com/2022/02/23/bC78YT.png)](https://imgtu.com/i/bC78YT)

#### Accept-Language

[![bC7w01.png](https://s4.ax1x.com/2022/02/23/bC7w01.png)](https://imgtu.com/i/bC7w01)

首部字段 Accept-Language 用来告知服务器用户代理能够处理的自然语言集（指中文或英文等），以及自然语言集的相对优先级。可一次指定多种自然语言集。

#### Authorization

[![bCHu9O.png](https://s4.ax1x.com/2022/02/23/bCHu9O.png)](https://imgtu.com/i/bCHu9O)

首部字段 Authorization 是用来告知服务器，用户代理的认证信息（证书值）。通常，想要通过服务器认证的用户代理会在接收到返回的
401 状态码响应后，把首部字段 Authorization 加入请求中。共用缓存在接收到含有 Authorization 首部字段的请求时的操作处理会略有差异



#### Expect

[![bCHwvQ.png](https://s4.ax1x.com/2022/02/23/bCHwvQ.png)](https://imgtu.com/i/bCHwvQ)



#### From

[![bCHR8U.png](https://s4.ax1x.com/2022/02/23/bCHR8U.png)](https://imgtu.com/i/bCHR8U)

首部字段 From 用来告知服务器使用用户代理的用户的电子邮件地址

#### Host

[![bCHO2D.png](https://s4.ax1x.com/2022/02/23/bCHO2D.png)](https://imgtu.com/i/bCHO2D)

首部字段 Host 会告知服务器，请求的资源所处的互联网主机名和端口号。Host 首部字段在 HTTP/1.1 规范内是唯一一个必须被包含在请
求内的首部字段

#### If-Match

[![bCbZrj.png](https://s4.ax1x.com/2022/02/23/bCbZrj.png)](https://imgtu.com/i/bCbZrj)

形如 If-xxx 这种样式的请求首部字段，都可称为条件请求。服务器接收到附带条件的请求后，只有判断指定条件为真时，才会执行请求。

#### If-Modified-Since

[![bCbUd1.png](https://s4.ax1x.com/2022/02/23/bCbUd1.png)](https://imgtu.com/i/bCbUd1)

#### If-None-Match

![2022-02-23-20-07-23.png](https://s2.loli.net/2022/02/23/lpAPnVjK3xawmHu.png)



#### If-Range

![](https://pic.imgdb.cn/item/621624382ab3f51d91f90fe0.png)



#### If-Unmodified-Since

#### Max-Forwards

#### Proxy-Authorization

#### Range

#### Referer

#### TE

#### User-Agent

User-Agent 用于传达浏览器的种类

>   Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36



### 响应首部字段

响应首部字段是由服务器端向客户端返回响应报文中所使用的字段，用于补充响应的附加信息、服务器信息，以及对客户端的附加要求等信息

![](https://pic.imgdb.cn/item/621625432ab3f51d91fbb428.png)



#### Accept-Ranges

![](https://pic.imgdb.cn/item/62162b582ab3f51d910b56aa.png)

首部字段 Accept-Ranges 是用来告知客户端服务器是否能处理范围请求，以指定获取服务器端某个部分的资源



#### Age

![](https://pic.imgdb.cn/item/62162ba02ab3f51d910c1ddf.png)

首部字段 Age 能告知客户端，源服务器在多久前创建了响应。字段的单位为秒。



#### ETag

![](https://pic.imgdb.cn/item/62162be32ab3f51d910ccc5e.png)

首部字段 ETag 能告知客户端实体标识。它是一种可将资源以字符串形式做唯一性标识的方式。服务器会为每份资源分配对应的 ETag值



#### Location

![](https://pic.imgdb.cn/item/62162c2f2ab3f51d910d8f34.png)

使用首部字段 Location 可以将响应接收方引导至某个与请求 URI 位置不同的资源

#### Proxy-Authenticate

#### Retry-After

#### Server

#### Vary

#### WWW-Authenticate



### 实体首部字段

实体首部字段是包含在请求报文和响应报文中的实体部分所使用的首部，用于补充内容的更新时间等与实体相关的信息。

在请求和响应两方的 HTTP 报文中都含有与实体相关的首部字段

#### Allow

![](https://pic.imgdb.cn/item/62162eb02ab3f51d9113eed0.png)

首部字段 Allow 用于通知客户端能够支持 Request-URI 指定资源的所有 HTTP 方法。

#### Content-Encoding

![](https://pic.imgdb.cn/item/62162eff2ab3f51d9114b10c.png)



#### Content-Language

![](https://pic.imgdb.cn/item/62162f2c2ab3f51d91152901.png)



#### Content-Length

![](https://pic.imgdb.cn/item/62162f562ab3f51d91159752.png)



#### Content-Location

![](https://pic.imgdb.cn/item/62162fd22ab3f51d9116dad9.png)



#### Content-MD5

![](https://pic.imgdb.cn/item/62162fff2ab3f51d91177411.png)



#### Content-Range

#### Content-Type

>Content-Type: text/html; charset=UTF-8

#### Expires

![](https://pic.imgdb.cn/item/621630542ab3f51d91184831.png)

#### Last-Modified

![](https://pic.imgdb.cn/item/621630782ab3f51d9118a5d4.png)





### 为 Cookie 服务的首部字段

管理服务器与客户端之间状态的 Cookie，虽然没有被编入标准化HTTP/1.1 的 RFC2616 中，但在 Web 网站方面得到了广泛的应用。Cookie 的工作机制是用户识别及状态管理。Web 网站为了管理用户状态会通过 Web 浏览器，把一些数据临时写入用户的计算机内。接着当用户访问该Web网站时，可通过通信方式取回之前发放的Cookie。

![](https://pic.imgdb.cn/item/621630e32ab3f51d9119b55c.png)





## 第七章：确保 Web 安全的HTTPS

在 HTTP 协议中有可能存在信息窃听或身份伪装等安全问题。使用HTTPS 通信机制可以有效地防止这些问题。本章我们就了解一下
HTTPS。

### HTTP 的缺点

1.  HTTP 主要有这些不足，例举如下。通信使用明文（不加密），内容可能会被窃听
2.  不验证通信方的身份，因此有可能遭遇伪装
3.  无法证明报文的完整性，所以有可能已遭篡改

#### 通信使用明文可能会被窃听

由于 HTTP 本身不具备加密的功能，所以也无法做到对通信整体（使用 HTTP 协议通信的请求和响应的内容）进行加密。即，HTTP 报文
使用明文（指未经过加密的报文）方式发送

![](https://pic.imgdb.cn/item/621632752ab3f51d911dbbdc.png)

![](https://pic.imgdb.cn/item/621632902ab3f51d911dfce0.png)



##### 加密处理防止被窃听

在目前大家正在研究的如何防止窃听保护信息的几种对策中，最为普及的就是加密技术。加密的对象可以有这么几个

==通信的加密==

一种方式就是将通信加密。HTTP 协议中没有加密机制，但可以通过和 SSL（Secure Socket Layer，安全套接层）或TLS（Transport Layer Security，安全层传输协议）的组合使用，加密 HTTP 的通信内容。
用 SSL建立安全通信线路之后，就可以在这条线路上进行 HTTP通信了。与 SSL组合使用的 HTTP 被称为 HTTPS（HTTP Secure，超文本传输安全协议）或 HTTP over SSL。

![](https://pic.imgdb.cn/item/621636cf2ab3f51d9128dab8.png)

==内容的加密==

还有一种将参与通信的内容本身加密的方式。由于 HTTP 协议中没有加密机制，那么就对 HTTP 协议传输的内容本身加密。即把
HTTP 报文里所含的内容进行加密处理。
在这种情况下，客户端需要对 HTTP 报文进行加密处理后再发送请求。

![](https://pic.imgdb.cn/item/621637662ab3f51d912a7152.png)

诚然，为了做到有效的内容加密，前提是要求客户端和服务器同时具备加密和解密机制。主要应用在 Web 服务中。有一点必须
引起注意，由于该方式不同于 SSL或 TLS 将整个通信线路加密处理，所以内容仍有被篡改的风险。

#### 不验证通信方的身份就可能遭遇伪装

HTTP 协议中的请求和响应不会对通信方进行确认。也就是说存在“服务器是否就是发送请求中 URI 真正指定的主机，返回的响应是否真的返回到实际提出请求的客户端”等类似问题。

##### 任何人都可发起请求

在 HTTP 协议通信时，由于不存在确认通信方的处理步骤，任何人都可以发起请求。另外，服务器只要接收到请求，不管对方是
谁都会返回一个响应（但也仅限于发送端的 IP 地址和端口号没有被 Web 服务器设定限制访问的前提下）。

![](https://pic.imgdb.cn/item/62163b0c2ab3f51d9134a201.png)

1.  HTTP 协议的实现本身非常简单，不论是谁发送过来的请求都会返回响应，因此不确认通信方，会存在以下各种隐患
2.  无法确定请求发送至目标的 Web 服务器是否是按真实意图返回响应的那台服务器。有可能是已伪装的 Web 服务器。
3.  无法确定响应返回到的客户端是否是按真实意图接收响应的那个客户端。有可能是已伪装的客户端。
4.  无法确定正在通信的对方是否具备访问权限。因为某些Web 服务器上保存着重要的信息，只想发给特定用户通信的权限。
5.  无法判定请求是来自何方、出自谁手。即使是无意义的请求也会照单全收。
6.  无法阻止海量请求下的 DoS 攻击（Denial of Service，拒绝服务攻击）。

##### 查明对手的证书

虽然使用 HTTP 协议无法确定通信方，但如果使用 SSL则可以。SSL不仅提供加密处理，而且还使用了一种被称为证书的手段，可用于确定对方


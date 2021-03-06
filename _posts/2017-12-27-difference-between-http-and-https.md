---
layout:     post
title:      HTTP和HTTPS的区别
subtitle:   difference-between-http-and-https
date:       2017-12-27
author:     Fe
header-img: img/post-bg-miku1.jpg
catalog: true
tags:
    - Http
    - Https
---
>参考:[百度](https://www.baidu.com),[豆豆蛙](https://www.cnblogs.com/wqhwe/p/5407468.html),[CTI论坛](https://zm12.sm-tc.cn/?src=l4uLj8XQ0IiIiNGci5aZkI2KktGckJLQ&from=derive&depth=2&link_type=60&v=1&uid=45e62ef48ddf9ed73f9b1e6f0963a9d4&hid=95f7e320871686ae155299f4ca084a43&restype=1&uc_param_str=dnntnwvepffrgibijbprsvdsdichei&query=http%E5%92%8Chttps%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)

## 前言

自从我购买了域名[fedemo.top](/),域名从[fedemo.github.io](/)变成[fedemo.top](/)后,我发现我的 *S* 不见了  

原来,我的网址是`https://fedemo.github.io`,前面还带一把绿色的锁   

现在,却只剩一个孤零零的`fedemo.top`,最关键的是前面的 *https* 不见了,浏览器还提示我说是不安全的网站   

作为一名有强迫症的程序猿,怎么能忍.这就动下了改造成https的念头   

这里记录一下查找资料,学习的过程

## 什么是HTTP

http(全称：Hypertext transfer protocol 超文本传输协议),是一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议。

## 什么是HTTPS

HTTPS（全称：Hypertext Transfer Protocol over Secure Socket Layer），是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。 它是一个URI scheme（抽象标识符体系），句法类同http:体系。用于安全的HTTP数据传输。https:URL表明它使用了HTTP，但HTTPS存在不同于HTTP的默认端口及一个加密/身份验证层（在HTTP与TCP之间）。这个系统的最初研发由网景公司进行，提供了身份验证与加密通讯方法，现在它被广泛用于万维网上安全敏感的通讯，例如交易支付方面。  

## TLS/SSL的实现机制  

我们看看TLS/SSL所实现的几个主要机制：   

1. 证书：通过第三方权威证书颁发机构（如VeriSign）验证和担保网站的身份，防止他人伪造网站身份与不知情的用户建立加密连接。
2. 密钥交换：通过公钥（非对称）加密在网站服务器和用户之间协商生成一个共同的会话密钥。
3. 会话加密：通过机制(2)协商的会话密钥，用对称加密算法对会话的内容进行加密。
4. 消息校验：通过消息校验算法来防止加密信息在传输过程中被篡改。   

通过上述机制，用户与网站之间的传输内容受到了保护，因此能够获得很高的安全性。不过，任何密码学手段都不是绝对安全的，上面几个机制中其实都存在可能的风险：   

1. 证书：如果有人伪造证书，浏览器会发出警告，提示用户该网站的证书可能是伪造的，应该停止访问，但如果你无视浏览器的警告，你的会话信息就有可能被伪造者窃取。此外，如果第三方证书颁发机构遭到攻击，攻击者窃取了已颁发的证书密钥，就可以伪造相应的网站证书，完全骗过浏览器的安全机制，这样的例子的确曾经发生过。
2. 密钥交换：RSA，这是一种最普遍使用的公钥加密算法，一般来说安全性很高。
3. 会话加密：AES-256(CBCMode），这是一种非常广泛使用的加密算法，采用256位密钥代表其安全性很高，如果采用128位密钥（AES-128）则安全性就差一些。
4. 消息校验：SHA1，这是一种散列算法，SHA1的安全性比MD5要好，但如果采用SHA256则安全性会更好一些。   

## 分享一个小故事   

上面很抽象是不是，我们用“传纸条”这个人人小时候都做过的事来形象的说明一下。

![](https://raw.githubusercontent.com/FeDemo/posts_img/master/2017-12-27-difference-between-http-and-https/1.png)

>HTTP   

　　假设你现在正坐在教室里上课，现在你非常想和走道旁的迷人的TA说一些话，一般这个时候你会用“传纸条”的方式来交流。而这个方式和TCP/IP协议基本的工作模式十分相像：

- 通过小动作引起对方注意；
- 对方以多种可能的方式（注视、肢体语言等）回应于你；
- 你确认对方感知到你后，将纸条传给对方；
- 对方阅读纸条；
- 对方给予你阅读后的反应；

　　怎么样，这个流程是不是很熟悉？   

　　如果你要传递纸条的TA距离你很远怎么办？HTTP协议就是指你在纸条上写明你要传给的TA是谁，或者TA的座位在哪，接着只需要途径的同学拿到纸条后根据纸条上的指示依次将纸条传过去就OK了。    

　　这个时候问题来了：途径的同学完全可以观看并知道你在纸条上写了什么。   

　　这就是HTTP传输所面临的问题之一：中间人攻击，指消息传递的过程中，处在传递路径上的攻击者可以嗅探或者窃听传输数据的内容。    

>HTTPS   

　　HTTPS针对这个问题，采用了“加密”的方式来解决。最著名原始的加密方法就是对称加密算法了，就是双方约定一个暗号，用什么字母替换什么字母之类的。现在一般采用一种叫AES（高级加密算法）的对称算法。   

　　对称加密算法既指加密和解密需要使用的密钥key是一样的。   

　　AES在数学上保证了，只要你使用的key足够长，破解几乎是不可能的（除非光子计算机造出来了）   

　　我们先假设在没有密钥key的情况下，密文是无法被破解的，然后再回到这个教室。你将用AES加密后的内容噌噌噌地写在了纸条上，正要传出去的时候你突然想到，TA没有key怎么解密内容呀，或者说，应该怎么把key给TA？   

　　如果把key也写在纸条上，那么中间人照样可以破解窃听纸条内容。也许在现实环境中你有其他办法可以把key通过某种安全的渠道送到TA的手里，但是互联网上的实现难度就比较大了，毕竟不管怎样，数据都要经过那些路由。   

　　于是聪明的人类发明了另一种加密算法——非对称加密算法。这种加密算法会生成两个密钥（key1和key2）。凡是key1加密的数据，key1自身不能解密，需要key2才能解密；凡事key2加密的数据，key2自身不能解密，只有key1才能解密。   

　　目前这种算法有很多中，最常用的是RSA。其基于的数学原理是：   

　　两个大素数的乘积很容易算，但是用这个乘积去算出是哪两个素数相乘就很复杂了。好在以目前的技术，分解大数的素因确实比较困难，尤其是当这个大数足够大的时候（通常使用2的10次方个二进制位那么大），就算是超级计算机，解密也需要非常长的时间。    

　　现在就把这种非对称加密的方法应用在我们教室传纸条的场景里。    

　　你在写纸条内容之前先用RSA技术生成了一对密钥k1和k2.    

　　你把k1用明文传了出去，路经也许有人会截取，但是没有用，k1加密的数据需要k2才可以破解，而k2在你自己手中。   

　　k1传到了目的人，目的人会去准备一个接下来准备用于对称加密（AES）的传输密钥key，然后用收到的k1把key加密，传给你。    

　　你用手上的k2解出key后，全教室只有你和你的目的人拥有这个对称加密的key，你们俩就可以尽情聊天不怕窃听啦~    

　　这里也许你会有问题，为什么不直接用非对称加密来加密信息，而是加密AES的key呢？因为非对称加密和解密的平均消耗时间比较长，为了节省时间提高效率，我们通常只是用它来交换密钥，而非直接传输数据。    

　　然而使用非对称加密真的可以防范中间人攻击吗？虽然看上去很安全，但是实际上却挡不住可恶的中间人攻击。    

　　假设你是A，你的目的地是B，现在要途径一个恶意同学M。中间人的恶意之处在于它会伪装成你的目标。    

　　当你要和B完成第一次密钥交换的时候，M把纸条扣了下来，假装自己是B并伪造了一个key，然后用你发来的k1加密了key发还给你。    

　　你以为你和B完成了密钥交换，实际上你是和M完成了密钥交换。        

　　同事M和B完成一次密钥交换，让B以为和A你完成了密钥交换。      

　　现在整体的加密流程变成了A（加密链接1）->M(明文)->B(加密链接2)的情况了，这时候M依然可以知道A和B传输的全部消息。        

　　这个时候就是体现HTTPS和传纸条的区别了。在教室里，你是和一位与你身份几乎对等的的对象来通信；而在访问网站时，对方往往是一个比较大（或者知名）的服务者，他们有充沛的资源，或许他们可以向你证明他们的合法性。       

　　此时我们需要引入一个非常权威的第三方，一个专门用来认证网站合法性的组织，可以叫做CA（Certificate Authority）。各个网站服务商可以向CA申请证书，使得他们在建立安全连接时可以带上CA的签名。而CA得安全性是由操作系统或者浏览器来认证的。         

　　你的Windows、Mac、Linux、Chrome、Safari等会在安装的时候带上一个他们认为安全的CA证书列表，只有和你建立安全连接的网站带有这些CA的签名，操作系统和浏览器才会认为这个链接是安全的，否则就有可能遭到中间人攻击。        

　　一旦某个CA颁发的证书被用于的非法途径，那么这个CA之前颁发过的所有证书都将被视为不安全的，这让所有CA在颁发证书时都十分小心，所以CA证书在通常情况下是值得信任的。

## HTTPS和HTTP的区别
HTTP协议传输的数据都是未加密的，也就是明文的，因此使用HTTP协议传输隐私信息非常不安全，为了保证这些隐私数据能加密传输，于是网景公司设计了SSL（Secure Sockets Layer）协议用于对HTTP协议传输的数据进行加密，从而就诞生了HTTPS。简单来说，HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全。

HTTPS和HTTP的区别主要如下：   

1. https协议需要到ca申请证书，一般免费证书很少，需要交费。

2. http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议

3. http的连接很简单，是无状态的 HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议 要比http协议安全 HTTPS解决的问题。

4. http和https使用的是完全不同的连接方式用的端口也不一样：前者是80，后者是443。


## HTTPS的优点

使用HTTPS协议进行通信,在安全性上确实会比HTTP高出一个级别,但是并不是无懈可击的.我们使用HTTPS,主要是为了以下好处:

1. 使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；

2. HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。

3. HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。

4. 谷歌曾在2014年8月份调整搜索引擎算法，并称“比起同等HTTP网站，采用HTTPS加密的网站在搜索结果中的排名将会更高”。

## HTTPS的缺点

当然,HTTP在安全性上比HTTP要高,那么必然的在性能上会有所折扣.还是存在不足之处的：

1. HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；

2. HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；

3. SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。

4. SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。

5. HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

## http切换到HTTPS

如果需要将网站从http切换到https到底该如何实现呢？

这里需要将页面中所有的链接，例如js，css，图片等等链接都由http改为https。例如：[http://www.baidu.com](/) 改为 [https://www.baidu.com](/)

BTW，这里虽然将http切换为了https，还是建议保留http。所以我们在切换的时候可以做http和https的兼容，具体实现方式是，去掉页面链接中的http头部，这样可以自动匹配http头和https头。例如：将[http://www.baidu.com](/)改为 [www.baidu.com](/)。然后当用户从http的入口进入访问页面时，页面就是http，如果用户是从https的入口进入访问页面，页面即使https的。

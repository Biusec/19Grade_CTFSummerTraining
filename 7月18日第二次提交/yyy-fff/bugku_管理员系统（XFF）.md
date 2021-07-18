### bugku_管理员系统（XFF）

打开界面又是非常的熟悉。管理员系统首先猜测admin和123456

失败了

看一下源代码，发现了一串base64编码的数据

![](https://pic.imgdb.cn/item/60efea125132923bf8c5b0d4.jpg)

解码发现是test123，看这样子猜测是密码

然后使用bp抓包，发现没有头绪，看题目应该是要伪造IP地址。看了下大佬们的评论，知道了有个XFF

>1.X-Forwarded-For:
>
>简称XFF头，它代表客户端，也就是HTTP的请求端真实的IP，只有在通过了HTTP 代理或者负载均衡服务器时才会添加该项。它不是RFC中定义的标准请求头信息，在squid缓存代理服务器开发文档中可以找到该项的详细介绍。标准格式如下：X-Forwarded-For: client1, proxy1, proxy2。
>
>这一HTTP头一般格式如下:
>
>X-Forwarded-For: client1, proxy1, proxy2, proxy3
>
>其中的值通过一个 逗号+空格 把多个IP地址区分开, 最左边(client1)是最原始客户端的IP地址, 代理服务器每成功收到一个请求，就把请求来源IP地址添加到右边。 在上面这个例子中，这个请求成功通过了三台代理服务器：proxy1, proxy2 及 proxy3。请求由client1发出，到达了proxy3(proxy3可能是请求的终点)。请求刚从client1中发出时，XFF是空的，请求被发往proxy1；通过proxy1的时候，client1被添加到XFF中，之后请求被发往proxy2;通过proxy2的时候，proxy1被添加到XFF中，之后请求被发往proxy3；通过proxy3时，proxy2被添加到XFF中，之后请求的的去向不明，如果proxy3不是请求终点，请求会被继续转发。
>
>如果一个 HTTP 请求到达服务器之前，经过了三个代理 Proxy1、Proxy2、Proxy3，IP 分别为 IP1、IP2、IP3，用户真实 IP 为 IP0，那么按照 XFF 标准，服务端最终会收到这样信息：X-Forwarded-For: IP0, IP1, IP2
>
>鉴于伪造这一字段非常容易，应该谨慎使用X-Forwarded-For字段。正常情况下XFF中最后一个IP地址是最后一个代理服务器的IP地址, 这通常是一个比较可靠的信息来源。
>
>2.Client-IP
>
>client的ip是通过http的头部发送到server端的
>
>比方，在打开网址www.baidu.com的时候。通过firebug能够看到请求头部，头部里包括client的信息，比方cookie等。
>
>3.remote_addr
>
>remote_addr代表客户端的IP,但它的值不是由客户端提供的,而是服务端根据客户端的ip指定的,当你的浏览器访问某个网站时,假设中间没有任何代理,那么网站的web服务器(Nginx,Apache等)就会把remote_addr设为你的机器IP,如果你用了某个代理,那么你的浏览器会先访问这个代理,然后再由这个代理转发到网站,这样web服务器就会把remote_addr设为这台代理机器的IP。
>
>4.X-Origining-IP
>
>维基百科的定义：
>
>X-Origining-IP（不要与X-Forwared-For混淆）电子邮件头字段是用于识别连接到邮件服务的HTTP前端的客户端的起始IP地址的事实标准。当客户机直接连接到邮件服务器时，服务器已经知道它的地址，但是web前端充当在内部连接到邮件服务器的代理。因此，尽管是前端，这个头也可以用来识别原始的发件人地址。

抓包然后修改内容：

![](https://pic.imgdb.cn/item/60efeabf5132923bf8ca82ac.jpg)

发送到repeater，得到flag

![](https://pic.imgdb.cn/item/60efeae95132923bf8cbab65.jpg)
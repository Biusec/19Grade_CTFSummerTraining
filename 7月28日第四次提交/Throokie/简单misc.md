# 1.后门查杀

下载是个压缩包，解压是个网站源码。

题目说是后门查杀，打开d盾。查杀到大马

![image-20210731115637161](%E7%AE%80%E5%8D%95misc.assets/image-20210731115637161.png)

打开该文件。搜索flag没有找到。搜索pass，发现pass很像flag

![image-20210731115721020](%E7%AE%80%E5%8D%95misc.assets/image-20210731115721020.png)

尝试提交flag{6ac45fb83b3bc355c024f5034b947dd3}。正确

# 2.大白

![image-20210731120523621](%E7%AE%80%E5%8D%95misc.assets/image-20210731120523621.png)

  看到提示，再看大白只有下半身。想到改图片高度

前四个字节是宽度，后四个字节是高度。改图片高度成宽度一样即可。下图是修改后的

![image-20210731120713678](%E7%AE%80%E5%8D%95misc.assets/image-20210731120713678.png)

![image-20210731120742949](%E7%AE%80%E5%8D%95misc.assets/image-20210731120742949.png)

# 3.基础破解

提示密码四位数字。ARCHPR破解

![image-20210731122635624](%E7%AE%80%E5%8D%95misc.assets/image-20210731122635624.png)

![image-20210731122703178](%E7%AE%80%E5%8D%95misc.assets/image-20210731122703178.png)

![image-20210731122724958](%E7%AE%80%E5%8D%95misc.assets/image-20210731122724958.png)



# 4.数据包中的线索

打开数据包，随便追踪tcp流

![image-20210731133501093](%E7%AE%80%E5%8D%95misc.assets/image-20210731133501093.png)

发现数据格式很像base64加密过的。把头和尾两行垃圾数据删除

![image-20210731133537045](%E7%AE%80%E5%8D%95misc.assets/image-20210731133537045.png)

![image-20210731133552353](%E7%AE%80%E5%8D%95misc.assets/image-20210731133552353.png)

这个网址解密得到一张图片

[解密网址](https://the-x.cn/base64)

![image-20210731133702381](%E7%AE%80%E5%8D%95misc.assets/image-20210731133702381.png)

![image-20210731133708125](%E7%AE%80%E5%8D%95misc.assets/image-20210731133708125.png)

# 5.G语言

解压，两个文件，一个代码文件，一个describe.md文件。

上网搜索G语言，发现是图形编程语言。看了下跟以前学的工控机器人编程语言原理差不多。

找到在线G语言网址

[G网址](https://ncviewer.com/)

![image-20210731144806715](%E7%AE%80%E5%8D%95misc.assets/image-20210731144806715.png)

因为是buuoj写的题。flag是

```
flag{3d_pr1nt3d_fl49}
```

# 6.荷兰宽带数据泄露

下载是bin文件。百度查找发现是路由器配置备份文件。

用该软件可以查看bin文件内容

[routepassview下载地址](https://www.iplaysoft.com/routerpassview.html)

![image-20210731161448571](%E7%AE%80%E5%8D%95misc.assets/image-20210731161448571.png)

flag{053700357621}


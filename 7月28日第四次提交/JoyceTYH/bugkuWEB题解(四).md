**1.Flask_FileUpload**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731113445545.png" alt="image-20210731113445545" style="zoom:50%;" />

###### 场景为：

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731113149393.png" alt="image-20210731113149393" style="zoom: 67%;" />

###### 查看源码，发现只允许jpg/png文件且提示说上传的文件会用python执行：

![image-20210731113640569](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731113640569.png)

###### 准备一个jpg文件,用记事本在里面写,上传后查看源码：

> import os
> os.system(‘ls /’)						查看菜单
>
> import os
> os.system(‘cat /flag’)				查看菜单下的flag文件



<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731114504724.png" alt="image-20210731114504724" style="zoom:80%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731113352400.png" alt="image-20210731113352400" style="zoom:67%;" />

[参照]: https://blog.csdn.net/like98k/article/details/82891778



-----



**2.程序员本地网站**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731124141910.png" alt="image-20210731124141910" style="zoom:67%;" />

###### 场景为：

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731115636481.png" alt="image-20210731115636481" style="zoom:80%;" />

###### burpsuit抓包：

![image-20210731120316524](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731120316524.png)

###### 加上：X-Forwarded-For: 127.0.0.1

> X-Forwarded-For: 简称XFF头，它代表客户端，也就是HTTP的请求端真实的IP，只有在通过了HTTP 代理或者负载均衡服务器时才会添加该项。

![image-20210731120243273](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731120243273.png)



-----



**3.各种绕过哟**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731130414482.png" alt="image-20210731130414482" style="zoom:50%;" />

###### 打开场景：

![image-20210731120756290](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731120756290.png)

###### 通过阅读php代码，发现只要使uname的sha1和值与passwd的sha1的值相等即可，但同时他们两个的值又不能相等，于是构造数组

![index](C:\Users\TYH\Desktop\index.png)

[参照]: https://blog.csdn.net/qq_42777804/article/details/81778380?utm_source=blogxgwz0



-----



**4.**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731215527992.png" alt="image-20210731215527992" style="zoom:67%;" />

###### 场景为：

![image-20210731131105579](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731131105579.png)

###### 访问1p.html，发现跳转到bugku页面

![image-20210731131236296](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731131236296.png)

###### 在访问1p.html时使用burpsuit抓包：

![image-20210731131554655](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731131554655.png)

###### HTML解码：

![image-20210731132703531](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731132703531.png)

###### URL解码：

![image-20210731132002765](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731132002765.png)

###### BASE64解码：

![image-20210731132113882](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731132113882.png)

###### URL解码：

![image-20210731132914759](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731132914759.png)

[参照]: https://blog.csdn.net/lshandel/article/details/118736402
[参照]: https://blog.csdn.net/qq_41333578/article/details/92759619
[参照]: https://www.cnblogs.com/0yst3r-2046/p/10766897.html



-----



**5.MD5**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731222433542.png" alt="image-20210731222433542" style="zoom:50%;" />

###### 场景为：

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731222330341.png" alt="image-20210731222330341" style="zoom:80%;" />

![image-20210731222309323](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731222309323.png)

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731222524492.png" alt="image-20210731222524492" style="zoom:67%;" />

------



**6.字符？正则？**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223425473.png" alt="image-20210731223425473" style="zoom:50%;" />

###### 场景为：

![image-20210731223505136](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223505136.png)

###### 参照：

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223130933.png" alt="image-20210731223130933" style="zoom:67%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223154682.png" alt="image-20210731223154682" style="zoom:67%;" />

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223226707.png" alt="image-20210731223226707" style="zoom:67%;" />

![image-20210731223344906](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223344906.png)

[参照]: https://blog.csdn.net/wssmiss/article/details/89343804



-----



**7.冬至红包**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731224509721.png" alt="image-20210731224509721" style="zoom:50%;" />

###### 场景为：

![image-20210731223742856](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223742856.png)

![image-20210731223847125](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731223847125.png)

[参照]: https://blog.csdn.net/weixin_51661807/article/details/118319590
[参照]: https://blog.csdn.net/qq_52671379/article/details/117963565



-----



**8.需要管理员**

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731225443711.png" alt="image-20210731225443711" style="zoom:50%;" />

###### 场景为：

<img src="C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731225519794.png" alt="image-20210731225519794" style="zoom:67%;" />

![image-20210731225237704](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731225237704.png)

![image-20210731225405141](C:\Users\TYH\AppData\Roaming\Typora\typora-user-images\image-20210731225405141.png)

[参照]: https://www.cnblogs.com/c0d1/p/14899398.html




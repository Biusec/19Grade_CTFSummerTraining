### buuoj_[极客大挑战 2019] Secret File

首先查看源代码，发现一个新地址

![](https://pic.imgdb.cn/item/60faabf45132923bf8faba7b.jpg)

点进去看看，查看源代码，又发现一个地址

![](https://pic.imgdb.cn/item/60faac285132923bf8fb3e4a.jpg)

再点进去看看发现好像直接跳转到另一个界面了，用bp抓包看看

![](https://pic.imgdb.cn/item/60faac6c5132923bf8fbe1bb.jpg)

发现一个新文件，访问看看，发现一串代码

![](https://pic.imgdb.cn/item/60faac935132923bf8fc449c.jpg)

> strstr(str1,str2) 函数用于判断字符串str2是否是str1的子串。如果是，则该函数返回str2在str1中首次出现的地址（返回内容包括str2出现后所有字符串）；否则，返回NULL。
>
> 
>
> stristr(str1，str2) 函数搜索字符串在另一字符串中的第一次出现（返回str2出现前的所有字符）。
>
> ​	该函数是二进制安全的。
>
> ​	该函数是不区分大小写的。如需进行区分大小写的搜索，请使用strstr()函数。

看这样子是文件包含，可以直接使用伪协议读取

```
secr3t.php?file=php://filter/read=convert.base64-encode/resource=flag.php
```

得到源代码的base64编码，解码即可

![](https://pic.imgdb.cn/item/60fab0255132923bf80509fa.jpg)
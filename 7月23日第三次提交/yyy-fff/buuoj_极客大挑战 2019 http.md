### buuoj_极客大挑战 2019 http

先看看源代码，发现了一个文件，点进去看看

![](https://pic.imgdb.cn/item/60fad6835132923bf89074ee.jpg)

发现好像要修改请求头

![](https://pic.imgdb.cn/item/60fad6ae5132923bf8915475.jpg)

抓包修改一下

```
Referer:https://www.Sycsecret.com
```

![](https://pic.imgdb.cn/item/60fad7585132923bf894d819.jpg)

看来还要修改一下

```
User-Agent:Syclover
```

![](https://pic.imgdb.cn/item/60fad7b25132923bf896b21e.jpg)

还要改一下，从本地访问

```
X-Forwarded-For:127.0.0.1
```

![](https://pic.imgdb.cn/item/60fad7f85132923bf8981d93.jpg)

完成，完整信息

![](https://pic.imgdb.cn/item/60fad8165132923bf898b5b1.jpg)
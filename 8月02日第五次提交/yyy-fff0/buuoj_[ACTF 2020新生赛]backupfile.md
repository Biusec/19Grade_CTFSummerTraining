### buuoj_[ACTF 2020新生赛]backupfile

看题目，找到备份文件，那先用dirsearch扫一扫

![](https://pic.imgdb.cn/item/6107e6305132923bf8718ebd.jpg)

![](https://pic.imgdb.cn/item/6107e6575132923bf871fd57.jpg)

flag.php里面是空的，把这个备份文件下载了，得到源码

![](https://pic.imgdb.cn/item/6107e6985132923bf872b7eb.jpg)

很明显，双等于号是弱比较直接构造playload

```
?key=123
```

得到flag


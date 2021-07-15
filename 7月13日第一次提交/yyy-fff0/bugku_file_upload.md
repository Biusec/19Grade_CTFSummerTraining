### bugku_file_upload

首先查看页面源代码，可以看见上传文件类型仅支持 jpg 和 png，而且可以知道是将文件按python解析执行的

![image.png](https://i.loli.net/2021/07/12/WUvwIiKxDBMLuyE.png)

在python中，system函数可以将字符串转换成命令在服务器上运行，一般在os.system上面封装运行

新建txt文件，内容输入

``` 
import os
os.system('cat/flag')
```

将文件后缀改为jpg或者png

上传之后不知道为什么报错了。。。修改文件内容：

```
import os
os.system('echo $flag$')
```

![image.png](https://i.loli.net/2021/07/12/NjQT1GX8quSYVk7.png)
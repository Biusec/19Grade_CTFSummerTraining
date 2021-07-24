# Bugku-13.NoString

1.由题目可以猜测出这道题可能没有明显的字符串，因此可能对字符串进行了加密，先运行程序，输出“please input u flage:”，用x32dbg打开，搜索字符串，果然没有。不过如果让程序运行到输入的中断时再搜索字符串时，就可以找到这段字符串从而定位程序的位置了。

![13-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/13.NoString/img/13-1.png)

2.用ida打开看一下加密代码，发现很简单，只是“yelhzl)`gy|})|)oehnl3”这个字符串各字符与9相异或，得到“please input u flage:”，且flag为“oehnl3r=<?=hF@CCGPt”这个字符串解密，写程序解密后得到falg：flage:{4564aOIJJNY}，换成题目要求的格式，flag为：flag{4564aOIJJNY}

![13-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/13.NoString/img/13-2.png)
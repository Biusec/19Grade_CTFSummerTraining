# Bugku-3.Easy_Re

1.首先运行程序，可以看到需要输入flag

![3-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/3.Easy_Re/img/3-1.png)

2.通过工具得知这是一个32位程序，x32dbg启动！然后就是搜索字符串，找输入那块的代码，由于我的x32dbg装了搜索中文的插件，因此可以找到中文的字符串，这里其实已经把flag搜出来了，就是第一行的DUTCTF{...}，推测这个程序是直接把输入和该字符串进行比较判断的，如果flag正确则输出"flag get√"，否则输出"flag不太对呦，再试试呗，加油呦"，测试下该flag，正确。

![3-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/3.Easy_Re/img/3-2.png)

3.也可以顺带看下汇编代码

![3-3-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/3.Easy_Re/img/3-3-1.png)

这里是把flag放到了[ebp-44]，然后下文：

![3-3-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/3.Easy_Re/img/3-3-2.png)

这里可以看出输入的内容在[ebp-24]，而后又把它们放到寄存器里，就可以基本确定是要逐字符的做比较了，具体的汇编实现就不再看了，flag: DUTCTF{We1c0met0DUTCTF}
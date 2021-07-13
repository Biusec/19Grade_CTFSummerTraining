# Bugku-8.love

比较正常的逆向题目

1.程序要求一个输入，查字符串得知正确输出"right flag!"，错误输出"wrong flag!"

![8-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/8.love/img/8-1.png)

2.通过字符串可以定位main函数地址：0xAA56E0，发现代码太长，于是转战IDA。该程序的代码模块在内存中的地址为0xAA1000(在x32dbg内存布局中可查看)，因此main函数在模块中的偏移为0xAA56E0 - 0xAA1000 = 0x46E0，该模块在IDA中的地址为0x411000，则main函数地址为0x411000 + 0x46E0 = 0x4156E0。找到函数后，f5生成伪代码如下：

![8-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/8.love/img/8-2.png)

3.通过分析得知：Dest是个char类型的数组，大小为100，Str为输入的字符串，v0为输入字符串的长度，v1是一个字符串，由sub_4110BE这个函数计算得到，参数有输入字符串和其长度，因此推断这个函数是要重点分析的算法函数，返回一个字符串。接着，程序将该字符串复制了0x28个字节到空字符数组Dest中，并获取Dest的字符串长度，再将每个字符加其位置索引的值，最后与Str2比较，相等则是正确的flag，其中，Str2程序已经给出，为"e3nifIH9b_C@n@dH"。因此可得未相加位置索引前的字符串为"e2lfbDB2ZV95b3V9"。程序大致流程如上，接下来重点分析sub_4110BE

4.得到该函数代码如下

![8-4-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/8.love/img/8-4-1.png)

![8-4-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/8.love/img/8-4-2.png)

![8-4-3](https://github.com/OWORD/ctfimg/raw/main/Bugku/8.love/img/8-4-3.png)

首先可以看出Dst是要返回的字符串，这里的算法实际上是base64加密算法，因此我们对"e2lfbDB2ZV95b3V9"进行解密，得到{i_l0ve_you}，flag：flag{i_l0ve_you}


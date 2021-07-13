# Bugku-1.入门逆向

**先挑个软柿子捏，该题目是真·入门题目，适合用于熟悉逆向流程和工具的使用**

1.运行程序，查看现象，发现它只是输出了一串字符串，没有要求输入

![1.1](https://github.com/OWORD/ctfimg/raw/main/Bugku/1.入门逆向/img/1-1.png)

2.用PE格式查看工具看一下是32位还是64位，以选择合适的调试工具，我使用的是PPEE，可以看到是32位

![1-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/1.入门逆向/img/1-2.png)

3.接下来用x32dbg打开，F9执行到程序的入口点，然后可以尝试搜索下输出的那串字符串：右键->搜索->当前区域->字符串，然后会发现该字符串赫然在列，双击它，跳转到引用的地方，根据注释就能发现flag了: flag{Re_1s_S0_C0OL}(注意区分0和O)

![1-3-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/1.入门逆向/img/1-3-1.png)

![1-3-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/1.入门逆向/img/1-3-2.png)

![1-3-3](https://github.com/OWORD/ctfimg/raw/main/Bugku/1.入门逆向/img/1-3-3.png)


# Bugku-5.Easy_vb

VB写的窗口程序，但是……还是换汤不换药

1.程序启动后，根据界面推测flag是改变这些数字所得的



![5-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/5.Easy_vb/img/5-1.png)

2.但实际上，x32dbg搜索字符串发现，flag就在程序里

![5-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/5.Easy_vb/img/5-2.png)

根据题目提示，把MCTF{}换成flag{}，搞定，falg：flag{N3t_Rev_1s_E4ay}


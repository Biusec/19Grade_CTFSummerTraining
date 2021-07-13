# Bugku-4.游戏过关

这道题比较有意思，是一个数学游戏，大致意思是有八盏暗着的灯，你可以输入整数n(1-8)，表示要改变第几盏灯的状态，当改变其状态时，其前一盏(n-1)与后一盏(n+1)的状态也会改变(1的前一盏是8，8的后一盏是1)，当你将所有的灯都点亮时，输出flag，但是逆向就很简单了

检查正确输出周围的代码，发现向内存中写了许多数据，因此推断flag就在其中

![4-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/4.游戏过关/img/4-1.png)

将运行点设置到该函数，运行，即可得到flag：zsctf{T9is_tOpic_1s_v5ry_int7resting_b6t_others_are_n0t}
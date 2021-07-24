# Bugku-14.ezfibon

1.这道题和NoString一样，搜不到字符串，运行到输入也搜不到，因此可以尝试在输出函数处下断点，如printf、puts、putchar等，该程序使用的是puts，这样就可以定位到相关的代码片段了

![14-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/14.ezfibon/img/14-1.png)

2.用ida打开，发现只有3个函数，看来程序段应该是被加密了，我们可以用x64dbg（该程序是64-bit的）将程序dump出来再用ida分析(注：dump功能在 插件->Scylla 中)。找到sub_401530，将调用的一些函数名改一下，得到程序如下

![14-2-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/14.ezfibon/img/14-2-1.png)

![14-2-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/14.ezfibon/img/14-2-2.png)

3.由程序可以看出该程序是对每个字符分别加上斐波那契数列从2开始，再加字符偏移，再余64后再加64，最终和字符串"dynvFU{m@^mctQmVS~wenr"比较，相等则为正确的flag，由此可以推导每个字符为

```c
int fibon[] = {2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368};
char str[] = "dynvFU{m@^mctQmVS~wenr";
int n[] = {1, 1, 1, 1, 2, 2, 1, 2, 3, 3, 5, 7, 10, 17, 26, 42, 67, 106, 132, 238, 369, 606};
for(int i = 0; i < 22; ++i)
{
	str[i] = (int)str[i] - 64 + n[i] * 64 - fibon[i] - i;
}
```

其中的n最为难求，我是借助自己写的程序一个一个试出来的

最终解得flag：bugku{So_Ez_Fibon@cci}
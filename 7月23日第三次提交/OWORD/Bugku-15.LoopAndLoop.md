# Bugku-15.LoopAndLoop

1.首先用jadx打开，分析代码可以得知，主要的检查代码为check，该函数为原生函数，在so库中，我们需要输入一串代码使得check(code, 99) == 1835996258，因此，最核心的问题就是分析check函数

![15-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/15.LoopAndLoop/img/15-1.png)

2.用apktool解包apk，将so库扔入ida，找到check函数，代码如下：

![15-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/15.LoopAndLoop/img/15-2-1.png)

可以看到该函数是根据check的第二个参数决定是要返回最终值还是继续调用check1，2，3函数的，check1，2，3这三个函数定义在Java层，代码如下：

![15-2-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/15.LoopAndLoop/img/15-2-2.png)

可以看出都是一些循环，呼应了本题的题目：Loop And Loop，check函数中的第二个函数为循环的次数，且仅在check函数中-1，当次数为1时，不再循环，得出最后的结果。这类题目我们可以用爆破尝试下

3.首先，抄上check，check1，check2，check3函数，其中check1，check2，check3是可以优化的，具体如下：

```c
unsigned int check(unsigned int v, unsigned int n);

unsigned int check1(unsigned int v, unsigned int n)
{
	return check(v + 4950, n);
}

unsigned int check2(unsigned int v, unsigned int n)
{
	if(n % 2)
		return check(v - 499500, n);
	else
		return check(v + 499500, n);
}

unsigned int check3(unsigned int v, unsigned int n)
{
	return check(v + 49995000, n);
}

unsigned int check(unsigned int v, unsigned int n)
{
	unsigned int(*checkn[3])(unsigned int, unsigned int);
	checkn[0] = check1;
	checkn[1] = check2;
	checkn[2] = check3;
	if(n - 1 <= 0)
		return v;
	else
		return checkn[2 * n % 3](v, n - 1);
}
```

然后就是爆破了，对于爆破的范围，我们没必要从0开始，可以自己先试上一些值，确定个大致的输入范围使得可以得到目标值，我这里使用的是200000000~400000000，最终结果很快就得到了：236492408，然后在手机上输入代码，得到flag为：alictf{Jan6N100p3r}


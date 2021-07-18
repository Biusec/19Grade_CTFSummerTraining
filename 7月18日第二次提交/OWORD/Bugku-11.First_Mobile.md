# Bugku-11.First_Mobile

1.jadx打开，观察代码，主要判断位于encode.check()函数中，代码很简单，两个循环其实可以合并成一个

![11-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/11.First_Mobile/img/11-1.png)

2.循环内对输入字符串和encode类内的数组进行了处理，最后得到的内容还需要和输入比较，因此可以利用遍历爆破得到flag，代码如下：

```cpp
#include <stdio.h>

int main(void)
{
    char table[] = {'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', '{', '}', '_'};
    char b[] = {23, 22, 26, 26, 25, 25, 25, 26, 27, 28, 30, 30, 29, 30, 32, 32};
    char temp[17] = {0};
    for (int i = 0; i < 16; i++)
    {
        for(int j = 0; j < 55; ++j)
        {
            temp[i] = (table[j] + b[i]) % 61;
            temp[i] = (temp[i] * 2) - i;
            if(temp[i] == table[j])
            {
                putchar(table[j]);
                break;
            }
        }
    }
    return 0;
}
```

运行得到flag，按照题目格式，flag为：XMAN{LOHILMNMLKHILKHI}


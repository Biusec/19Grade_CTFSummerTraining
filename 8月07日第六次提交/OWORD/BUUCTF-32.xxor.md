# BUUCTF-32.xxor

1.用IDA打开，查看代码，发现程序要求输入6个数字，并用函数sub_400686对每两个数字进行处理，最后用sub_400770判断结果，先查看判断结果的函数，代码如下：

```c
__int64 __fastcall sub_400770(_DWORD *a1)
{
  __int64 result; // rax

  if ( a1[2] - a1[3] == 2225223423LL
    && a1[3] + a1[4] == 4201428739LL
    && a1[2] - a1[4] == 1121399208LL
    && *a1 == -548868226
    && a1[5] == -2064448480
    && a1[1] == 550153460 )
  {
    puts("good!");
    result = 1LL;
  }
  else
  {
    puts("Wrong!");
    result = 0LL;
  }
  return result;
}
```

通过该函数可以知道经过处理后的6个数字分别为：

```c
num[0] = 3746099070
num[1] = 550153460
num[2] = 3774025685
num[3] = 1548802262
num[4] = 2652626477
num[5] = 2230518816
```

2.接下来查看sub_400686这个处理函数，其参数2是一个数组，内容为{2, 3, 3, 4}，代码如下：

```c
__int64 __fastcall sub_400686(unsigned int *a1, _DWORD *a2)
{
  __int64 result; // rax
  unsigned int v3; // [rsp+1Ch] [rbp-24h]
  unsigned int v4; // [rsp+20h] [rbp-20h]
  int v5; // [rsp+24h] [rbp-1Ch]
  unsigned int i; // [rsp+28h] [rbp-18h]

  v3 = *a1;
  v4 = a1[1];
  v5 = 0;
  for ( i = 0; i <= 0x3F; ++i )
  {
    v5 += 1166789954;
    v3 += (v4 + v5 + 11) ^ ((v4 << 6) + *a2) ^ ((v4 >> 9) + a2[1]) ^ 0x20;
    v4 += (v3 + v5 + 20) ^ ((v3 << 6) + a2[2]) ^ ((v3 >> 9) + a2[3]) ^ 0x10;
  }
  *a1 = v3;
  result = v4;
  a1[1] = v4;
  return result;
}
```

for循环内就是处理的主要程序，我们可以将循环逆序，得到解密程序

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
int data[] = {2, 2, 3, 4};

void re(unsigned int* num)
{
	unsigned int v3 = num[0];
	unsigned int v4 = num[1];
	unsigned int v5 = 1166789954 * (0x3f + 1);
	for(int i = 0x3f; i >= 0; --i)
	{
		v4 -= (v3 + v5 + 20) ^ ((v3 << 6) + data[2]) ^ ((v3 >> 9) + data[3]) ^ 0x10;
		v3 -= (v4 + v5 + 11) ^ ((v4 << 6) + data[0]) ^ ((v4 >> 9) + data[1]) ^ 0x20;
		v5 -= 1166789954;
	}
	num[0] = v3;
	num[1] = v4;
}

int main(void)
{
	unsigned int flag[6] = {3746099070, 550153460, 3774025685, 1548802262, 2652626477, 2230518816};
	for (int i = 0; i < 5; i += 2)
	{
		re(&flag[i]);
	}
	for (int i = 0; i < 6; i++)
		printf("%c%c%c", *((char*)&flag[i]+2), *((char*)&flag[i] + 1), *(char*)&flag[i]);
	return 0;
}
```

运行得到flag：flag{re_is_great!}


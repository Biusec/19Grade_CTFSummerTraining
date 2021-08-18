# BUUCTF-44.easystrcmp

1.IDA 打开，main函数如下

```c
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  if ( a1 > 1 )
  {
    if ( !strcmp(a2[1], "zer0pts{********CENSORED********}") )
      puts("Correct!");
    else
      puts("Wrong!");
  }
  else
  {
    printf("Usage: %s <FLAG>\n", *a2);
  }
  return 0LL;
}
```

只是与

```
zer0pts{********CENSORED********}
```

简单的字符比较，因此可能在之前对输入进行了处理

2.翻翻函数列表，发现了这样一个函数：

```c
__int64 __fastcall sub_6EA(__int64 a1, __int64 a2)
{
  int i; // [rsp+18h] [rbp-8h]
  int v4; // [rsp+18h] [rbp-8h]
  int j; // [rsp+1Ch] [rbp-4h]

  for ( i = 0; *(_BYTE *)(i + a1); ++i )
    ;
  v4 = (i >> 3) + 1;
  for ( j = 0; j < v4; ++j )
    *(_QWORD *)(8 * j + a1) -= qword_201060[j];
  return qword_201090(a1, a2);
}
```

其中qword_201060 = {0, 0x410A4335494A0942, 0x0B0EF2F50BE619F0, 0x4F0A3A064A35282B, 0}，该函数看起来是处理flag的，它将flag每8个字符分为一组，然后减去一个qword的数，因此如果要得到flag，应该加上这个数字，稍微改造下就可得到解密程序：

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
typedef unsigned long long QWORD;
QWORD data[] = {0, 0x410A4335494A0942, 0x0B0EF2F50BE619F0, 0x4F0A3A064A35282B, 0};

void test(char* str)
{
	int i, j, v4;
	for(i = 0; str[i]; ++i)
		;
	v4 = (i >> 3) + 1;
	for(j = 0; j < v4; ++j)
		*(QWORD*)(&str[8 * j]) += data[j];
}

int main(void)
{
	char flag[] = "zer0pts{********CENSORED********}";
	test(flag);
	printf("%s", flag);
	return 0;
}
```

运行得到flag：zer0pts{l3ts_m4k3_4_DETOUR_t0d4y}
# BUUCTF-29.Level1

1.这道题有一个output文本文件，打开是19个数字，用IDA打开程序，发现很简洁，代码如下

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+4h] [rbp-2Ch]
  FILE *stream; // [rsp+8h] [rbp-28h]
  char ptr[24]; // [rsp+10h] [rbp-20h] BYREF
  unsigned __int64 v7; // [rsp+28h] [rbp-8h]

  v7 = __readfsqword(0x28u);
  stream = fopen("flag", "r");
  fread(ptr, 1uLL, 0x14uLL, stream);
  fclose(stream);
  for ( i = 1; i <= 19; ++i )
  {
    if ( (i & 1) != 0 )
      printf("%ld\n", (unsigned int)(ptr[i] << i));
    else
      printf("%ld\n", (unsigned int)(i * ptr[i]));
  }
  return 0;
}
```

2.代码主要意思就是读入flag文本文件，然后奇数输出其左移i位的数，偶数输出其i倍的数，i为字符串的索引，我们根据这条很容易就可以写出脚本：

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

void test(char* ptr)
{
	int out[] = {198, 232, 816, 200, 1536, 300, 6144, 984, 51200, 570, 92160, 1200, 565248, 756, 1474560, 800, 6291456, 1782, 65536000};
	for(int i = 1; i <= 19; ++i)
	{
		if((i & 1) != 0)
			ptr[i - 1] = out[i - 1] >> i; //奇数
		else
			ptr[i - 1] = out[i - 1] / i; //偶数
	}
}

int main(void)
{
	char ptr[256] = {0};
	test(ptr);
	printf("%s", ptr);
	return 0;
}
```

运行得到flag：ctf2020{d9-dE6-20c}
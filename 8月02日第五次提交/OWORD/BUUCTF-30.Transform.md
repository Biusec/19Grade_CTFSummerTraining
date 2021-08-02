# BUUCTF-30.Transform

1.IDA打开，f5，代码如下：

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char Str[104]; // [rsp+20h] [rbp-70h] BYREF
  int j; // [rsp+88h] [rbp-8h]
  int i; // [rsp+8Ch] [rbp-4h]

  sub_402230(argc, argv, envp);
  printf("Give me your code:\n");
  scanf("%s", Str);
  if ( strlen(Str) != 33 )
  {
    printf("Wrong!\n");
    system("pause");
    exit(0);
  }
  for ( i = 0; i <= 32; ++i )
  {
    byte_414040[i] = Str[dword_40F040[i]];
    byte_414040[i] ^= LOBYTE(dword_40F040[i]);
  }
  for ( j = 0; j <= 32; ++j )
  {
    if ( byte_40F0E0[j] != byte_414040[j] )
    {
      printf("Wrong!\n");
      system("pause");
      exit(0);
    }
  }
  printf("Right!Good Job!\n");
  printf("Here is your flag: %s\n", Str);
  system("pause");
  return 0;
}
```

2.可以看出代码逻辑就是简单的换位再与，解密程序如下：

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
char data[] = "gy{\x7f""u+<RSyW^]B{-*fB~LWyAk~e<\\EobM";
char data2[] = {0x09, 0x0A, 0x0F, 0x17, 0x07, 0x18, 0x0C, 0x06, 0x01, 0x10, 0x03, 0x11, 0x20, 0x1D, 0x0B, 0x1E, 0x1B, 0x16, 0x04, 0x0D, 0x13, 0x14, 0x15, 0x02, 0x19, 0x05, 0x1F, 0x08, 0x12, 0x1A, 0x1C, 0x0E, 0x00};

void test(char* flag)
{
	for(int i = 0; i < 33; ++i)
		data[i] ^= data2[i];
	for(int i = 0; i < 33; ++i)
		flag[data2[i]] = data[i];
}

int main(void)
{
	char flag[256] = {0};
	test(flag);
	printf("%s", flag);
	return 0;
}
```

运行得到flag：MRCTF{Tr4nsp0sltiON_Clph3r_1s_3z}
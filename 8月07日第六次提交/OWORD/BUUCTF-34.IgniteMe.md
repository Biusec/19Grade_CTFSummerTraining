# BUUCTF-34.IgniteMe

1.IDA打开，f5，观察代码，

```c
void __noreturn start()
{
  DWORD NumberOfBytesWritten; // [esp+0h] [ebp-4h] BYREF

  NumberOfBytesWritten = 0;
  hFile = GetStdHandle(0xFFFFFFF6);
  dword_403074 = GetStdHandle(0xFFFFFFF5);
  WriteFile(dword_403074, aGiveMeTheFlag, 0x13u, &NumberOfBytesWritten, 0);
  sub_4010F0(NumberOfBytesWritten);
  if ( sub_401050() )
    WriteFile(dword_403074, aGoodJob, 0xAu, &NumberOfBytesWritten, 0);
  else
    WriteFile(dword_403074, aNotTooHotR, 0x24u, &NumberOfBytesWritten, 0);
  ExitProcess(0);
}
```

可以看出sub_4010F0为处理函数，sub_401050为检查函数

2.处理函数代码如下：

```c
int sub_4010F0()
{
  unsigned int v0; // eax
  char Buffer[260]; // [esp+0h] [ebp-110h] BYREF
  DWORD NumberOfBytesRead; // [esp+104h] [ebp-Ch] BYREF
  unsigned int i; // [esp+108h] [ebp-8h]
  char v5; // [esp+10Fh] [ebp-1h]

  v5 = 0;
  for ( i = 0; i < 0x104; ++i )
    Buffer[i] = 0;
  ReadFile(hFile, Buffer, 0x104u, &NumberOfBytesRead, 0);
  for ( i = 0; ; ++i )
  {
    v0 = sub_401020(Buffer);
    if ( i >= v0 )
      break;
    v5 = Buffer[i];
    if ( v5 != 10 && v5 != 13 )
    {
      if ( v5 )
        byte_403078[i] = v5;
    }
  }
  return 1;
}
```

本函数做的工作很简单，先将输入复制到Buffer中，sub_401020函数的作用是获取字符串长度，因此，后面仅仅是将非换行，回车和非零的字符复制到byte_403078中。

3.检查函数代码如下：

```c
int sub_401050()
{
  int v1; // [esp+0h] [ebp-Ch]
  int i; // [esp+4h] [ebp-8h]
  unsigned int j; // [esp+4h] [ebp-8h]
  char v4; // [esp+Bh] [ebp-1h]

  v1 = sub_401020(byte_403078);
  v4 = sub_401000();
  for ( i = v1 - 1; i >= 0; --i )
  {
    byte_403180[i] = v4 ^ byte_403078[i];
    v4 = byte_403078[i];
  }
  for ( j = 0; j < 0x27; ++j )
  {
    if ( byte_403180[j] != (unsigned __int8)byte_403000[j] )
      return 0;
  }
  return 1;
}
```

本函数中的sub_401000返回固定值0x700004，转为char就是4，byte_403000是一个char类型的数组，可以从内存里找到，该函数就是简单的循环异或再和数组比较，从而可编写解密程序如下：

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

char data[] = {0x0D, 0x26, 0x49, 0x45, 0x2A, 0x17, 0x78, 0x44, 0x2B, 0x6C, 0x5D, 0x5E, 0x45, 0x12, 0x2F, 0x17, 0x2B, 0x44, 0x6F, 0x6E, 0x56, 0x09, 0x5F, 0x45, 0x47, 0x73, 0x26, 0x0A, 0x0D, 0x13, 0x17, 0x48, 0x42, 0x01, 0x40, 0x4D, 0x0C, 0x02, 0x69};

char xorn = 0x04;

int main(void)
{
	char flag[40] = {0};
	for(int i = 38; i >=0; --i)
	{
		flag[i] = xorn ^ data[i];
		xorn = flag[i];
	}
	printf("%s", flag);
	return 0;
}
```

运行获得flag：R_y0u_H0t_3n0ugH_t0_1gn1t3@flare-on.com
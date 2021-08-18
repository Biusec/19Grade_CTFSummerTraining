# BUUCTF-48.signal

1.IDA打开，main函数如下：

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v4[117]; // [esp+18h] [ebp-1D4h] BYREF

  __main();
  qmemcpy(v4, &unk_403040, 0x1C8u);
  vm_operad(v4, 114);
  puts("good,The answer format is:flag {}");
  return 0;
}
```

先是从unk_403040这个地址copy了0x1C8u大小的数据，dump出来，得到以下qword数据：

```c
QWORD data = {10, 4, 16, 8, 3, 5, 1, 4, 32, 8, 5, 3, 1, 3, 2, 8, 11, 1, 12, 8, 4, 4, 1, 5, 3, 8, 3, 33, 1, 11, 8, 11, 1, 4, 9, 8, 3, 32, 1, 2, 81, 8, 4, 36, 1, 12, 8, 11, 1, 5, 2, 8, 2, 37, 1, 2, 54, 8, 4, 65, 1, 2, 32, 8, 5, 1, 1, 5, 3, 8, 2, 37, 1, 4, 9, 8, 3, 32, 1, 2, 65, 8, 12, 1}
```

然后进入vm_operad查看，看名字似乎和虚拟机相关

```c
int __cdecl vm_operad(int *a1, int a2)
{
  int result; // eax
  char Str[200]; // [esp+13h] [ebp-E5h] BYREF
  char v4; // [esp+DBh] [ebp-1Dh]
  int v5; // [esp+DCh] [ebp-1Ch]
  int v6; // [esp+E0h] [ebp-18h]
  int v7; // [esp+E4h] [ebp-14h]
  int v8; // [esp+E8h] [ebp-10h]
  int v9; // [esp+ECh] [ebp-Ch]

  v9 = 0;
  v8 = 0;
  v7 = 0;
  v6 = 0;
  v5 = 0;
  while ( 1 )
  {
    result = v9;
    if ( v9 >= a2 )
      return result;
    switch ( a1[v9] )
    {
      case 1:
        Str[v6 + 100] = v4;
        ++v9;
        ++v6;
        ++v8;
        break;
      case 2:
        v4 = a1[v9 + 1] + Str[v8];
        v9 += 2;
        break;
      case 3:
        v4 = Str[v8] - LOBYTE(a1[v9 + 1]);
        v9 += 2;
        break;
      case 4:
        v4 = a1[v9 + 1] ^ Str[v8];
        v9 += 2;
        break;
      case 5:
        v4 = a1[v9 + 1] * Str[v8];
        v9 += 2;
        break;
      case 6:
        ++v9;
        break;
      case 7:
        if ( Str[v7 + 100] != a1[v9 + 1] )
        {
          printf("what a shame...");
          exit(0);
        }
        ++v7;
        v9 += 2;
        break;
      case 8:
        Str[v5] = v4;
        ++v9;
        ++v5;
        break;
      case 10:
        read(Str);
        ++v9;
        break;
      case 11:
        v4 = Str[v8] - 1;
        ++v9;
        break;
      case 12:
        v4 = Str[v8] + 1;
        ++v9;
        break;
      default:
        continue;
    }
  }
}
```

嗯……是一个很大的switch，以我们得到的data来判断执行的顺序，可以得知执行顺序如下：

```c
{10, 4, 8, 3, 1, 4, 8, 5, 1, 3, 8, 11, 1, 12, 8, 4, 1, 5, 8, 3, 1, 11, 8, 11, 1, 4, 8, 3, 1, 2, 8, 4, 1, 12, 8, 11, 1, 5, 8, 2, 1, 2, 8, 4, 1, 2, 8, 5, 1, 5, 8, 2, 1, 4, 8, 3, 1, 2, 8, 12, 1, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7}
```

其中，case10中调用了一个read函数，代码如下

```c
size_t __cdecl read(char *Str)
{
  size_t result; // eax

  printf("string:");
  scanf("%s", Str);
  result = strlen(Str);
  if ( result != 15 )
  {
    puts("WRONG!\n");
    exit(0);
  }
  return result;
}
```

看来这个函数就是读取输入的，并判断其长度是否等于15，回到vm函数，观察switch，发现当case7时，程序判断str [100+v7]的值和某个值是否相等，而在case1时，程序向str[100+v6]写入值，而其他case都是做一些运算，结合程序执行顺序，可以猜测，每4个case，根据一位flag字符算出一个值，存在str[100+]的位置，而算出的值我们很容易可以得到：

```c
{34, 63, 52, 50, 115, 51, 24, 167, 49, 241, 40, 132, 193, 30, 122}
```

由此，我们就可以将vm函数逆序，算出flag，具体python脚本如下：

```python
data = [10, 4, 16, 8, 3, 5, 1, 4, 32, 8, 5, 3, 1, 3, 2, 8, 11, 1, 12, 8, 4, 4, 1, 5, 3, 8, 3, 33, 1, 11, 8, 11, 1, 4, 9, 8, 3, 32, 1, 2, 81, 8, 4, 36, 1, 12, 8, 11, 1, 5, 2, 8, 2, 37, 1, 2, 54, 8, 4, 65, 1, 2, 32, 8, 5, 1, 1, 5, 3, 8, 2, 37, 1, 4, 9, 8, 3, 32, 1, 2, 65, 8, 12, 1]
sort = [4, 8, 3, 1, 4, 8, 5, 1, 3, 8, 11, 1, 12, 8, 4, 1, 5, 8, 3, 1, 11, 8, 11, 1, 4, 8, 3, 1, 2, 8, 4, 1, 12, 8, 11, 1, 5, 8, 2, 1, 2, 8, 4, 1, 2, 8, 5, 1, 5, 8, 2, 1, 4, 8, 3, 1, 2, 8, 12, 1, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7]
sort.reverse()
code = [34, 63, 52, 50, 115, 51, 24, 167, 49, 241, 40, 132, 193, 30, 122]
v5 = 15
v6 = 15
v7 = 15
v8 = 15
v9 = 114
s = [0 for i in range(15)]
v4 = 0
for i in sort:
    if i == 1:
        v8 -= 1
        v6 -= 1
        v9 -= 1
        v4 = code[v6]
    elif i == 2:
        v9 -= 2
        s[v8] = chr(v4 - data[v9 + 1])
    elif i == 3:
        v9 -= 2
        s[v8] = chr(v4 + data[v9 + 1])
    elif i == 4:
        v9 -= 2
        s[v8] = chr(v4 ^ data[v9 + 1])
    elif i == 5:
        v9 -= 2
        s[v8] = chr(v4 // data[v9 + 1])
    elif i == 6:
        v9 -= 1
    elif i == 7:
        v9 -= 2
        v7 -= 1
    elif i == 8:
        v5 -= 1
        v9 -= 1
        v4 = ord(s[v5])
    elif i == 10:
        v9 -= 1
    elif i == 11:
        v9 -= 1
        s[v8] = chr(v4 + 1)
    elif i == 12:
        v9 -= 1
        s[v8] = chr(v4 - 1)

print(s)
```

得到flag：flag{757515121f3d478}
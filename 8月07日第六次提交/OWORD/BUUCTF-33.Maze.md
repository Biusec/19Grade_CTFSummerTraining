# BUUCTF-33.Maze

1.查壳发现有upx壳，脱壳后用ida打开，发现有错误

![33-1](https://github.com/OWORD/ctfimg/raw/main/BUUCTF/33.Maze/img/33-1.png)

2.用x32dbg打开，单步调试，发现有一个花指令E8

![33-2](https://github.com/OWORD/ctfimg/raw/main/BUUCTF/33.Maze/img/33-2.png)

可以用nop填充，然后再dump出来，ida继续分析

3.得到代码如下

```c
int sub_401000()
{
  int v0; // ecx
  int v2; // [esp-4h] [ebp-28h]
  int i; // [esp+10h] [ebp-14h]
  char v4[16]; // [esp+14h] [ebp-10h] BYREF

  sub_401140(aGoThroughTheMa);
  v2 = sub_401129("%14s", v4);
  if ( (v0 ^ v2) == v0 )
    __debugbreak();
  for ( i = 0; i <= 13; ++i )
  {
    switch ( v4[i] )
    {
      case 'a':
        --dword_408078;
        break;
      case 'd':
        ++dword_408078;
        break;
      case 's':
        --dword_40807C;
        break;
      case 'w':
        ++dword_40807C;
        break;
      default:
        continue;
    }
  }
  if ( dword_408078 == 5 && dword_40807C == -4 )
  {
    sub_401140(aCongratulation);
    sub_401140(aHereIsTheFlagF);
  }
  else
  {
    sub_401140(aTryAgain);
  }
  return 0;
}
```

可以看出这是道迷宫题，dword_408078和dword_40807C初始值为7和0，需要走14步，但是符合结果的路径有很多，因此肯定还有地图。在内存中可以如下地图：

```
*******+**
******* **
****    **
**   *****
** **F****
**    ****
**********
```

从而可知flag为：flag{ssaaasaassdddw}
# BUUCTF-26.Youngter-drive

1.先查壳，发现有upx壳，脱壳后程序无法运行，但网上的wp说可以用ida打开。（也可以用dump）打开后发现程序中有创建线程和互斥锁的代码，因此认定这可是一个多线程程序，主要线程有两条，代码分别如下：

```c
void __stdcall sub_E21A40(int a1)
{
  while ( 1 )
  {
    WaitForSingleObject(dword_E281B0, -1);
    CheckStackBalance();
    if ( dword_E28008 > -1 )
    {
      sub_E2112C(&input, dword_E28008);
      --dword_E28008;
      Sleep(100);
      CheckStackBalance();
    }
    ReleaseMutex(dword_E281B0);
    CheckStackBalance();
  }
}
```

```c
void __stdcall sub_E21B10(int a1)
{
  while ( 1 )
  {
    WaitForSingleObject(dword_E281B0, -1);
    CheckStackBalance();
    if ( dword_E28008 > -1 )
    {
      Sleep(100);
      CheckStackBalance();
      --dword_E28008;
    }
    ReleaseMutex(dword_E281B0);
    CheckStackBalance();
  }
}
```

2.可以看出两条线程是在交替执行的，其中一条调用了sub_E2112C，该函数是对输入的某个字符串加密的，所以整体是隔字符加密的，最终会与字符串“TOiZiZtOrYaToUwPnToBsOaOapsyS”比较，将[a-Z]送入sub_E2112C加密得到“WERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm”，其中字母Z没有对应的加密字符。隔字符替换，得到flag为“ThisisthreadofwindowshahaIsES”，由于dword_E281B0初始值为29，因此实际上需要30个字符才是正确的flag，网上wp给出正确的flag为“ThisisthreadofwindowshahaIsESE”
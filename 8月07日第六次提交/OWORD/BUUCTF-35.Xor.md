# BUUCTF-35.Xor

1.IDA打开，f5报错：call analysis failed，调用sub_401020函数时报的错，其报错原因主要有两种：

1.  IDA无法识别出正确的调用约定(calling convention)；
2.  DA无法识别出正确的参数个数。

这里应该是第二种，该函数目测是printf，但是参数却为(int, char)，跳到该函数，点击“Y”修改参数列表为(char*)，跳回，再次f5，成功

2.得到代码如下：

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  unsigned int i; // eax

  printf("Give Me Your Flag String:\n");
  scanf("%s", input);
  if ( strlen(input) != 27 )
  {
LABEL_6:
    printf("Wrong!\n");
    sub_404B7E("pause");
    _loaddll(0);
    __debugbreak();
  }
  for ( i = 0; i < 0x1B; ++i )
  {
    if ( ((unsigned __int8)i ^ (unsigned __int8)input[i]) != byte_41EA08[i] )
      goto LABEL_6;
  }
  printf("Right!\n");
  sub_404B7E("pause");
  return 0;
}
```

可以看出是很简单的异或题，byte_41EA08为“MSAWB~FXZ:J:`tQJ"N@ bpdd}8g”，解密后flag为MRCTF{@_R3@1ly_E2_R3verse!}
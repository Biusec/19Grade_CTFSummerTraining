# BUUCTF-39.Overlong

1.IDA打开，f5，该程序很简洁

```c
int __stdcall start(int a1, int a2, int a3, int a4)
{
  CHAR Text[128]; // [esp+0h] [ebp-84h] BYREF
  int v6; // [esp+80h] [ebp-4h]

  v6 = sub_401160(Text, &unk_402008, 28);
  Text[v6] = 0;
  MessageBoxA(0, Text, Caption, 0);
  return 0;
}
```

程序的主要思路就是通过sub_401160解码unk_402008处的数据，但只解码一部分，flag就在后面一部分，可以在动态调试时修改传入的数据地址，从而解码后面的内容，得到flag为：flag{I_a_M_t_h_e_e_n_C_o_D_i_n_g@flare-on.com}


# BUUCTF-37.level3

1.IDA打开，f5，嗯，是道base64题目，且直接给出了编码后的字符串"d2G0ZjLwHjS7DmOzZAY0X2lzX3CoZV9zdNOydO9vZl9yZXZlcnGlfD=="，且无法解码，应该是道变表的base64，而字符串中只有正常的表，因此程序中一定存在着变表的操作。

2.通过表的引用可以找到变表函数：

```c
__int64 O_OLookAtYou()
{
  __int64 result; // rax
  char v1; // [rsp+1h] [rbp-5h]
  int i; // [rsp+2h] [rbp-4h]

  for ( i = 0; i <= 9; ++i )
  {
    v1 = base64_table[i];
    base64_table[i] = base64_table[19 - i];
    result = 19 - i;
    base64_table[result] = v1;
  }
  return result;
}
```

变表后为“TSRQPONMLKJIHGFEDCBAUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/”，解码得到flag：wctf2020{Base64_is_the_start_of_reverse}
# BUUCTF-40.Cr0ssfun

1.本题没有什么难度，只是函数嵌套的逐字符判断，flag为：flag{cpp_@nd_r3verse_@re_fun}

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char v4[48]; // [rsp+0h] [rbp-30h] BYREF

  puts(" _    _ _   _ _____ _____   _____           ");
  puts("| |  | | | | /  ___|_   _| /  ___|          ");
  puts("| |  | | | | \\ `--.  | |   \\ `--.  ___  ___ ");
  puts("| |/\\| | | | |`--. \\ | |    `--. \\/ _ \\/ __|");
  puts("\\  /\\  / |_| /\\__/ / | |   /\\__/ /  __/ (__ ");
  puts(" \\/  \\/ \\___/\\____/  \\_/   \\____/ \\___|\\___|");
  while ( 1 )
  {
    puts("Input the flag");
    __isoc99_scanf("%s", v4);
    if ( (unsigned int)check((__int64)v4) == 1 )
      break;
    puts("0ops, your flag seems fake.");
    puts("==============================");
    rewind(_bss_start);
  }
  puts("Your flag is correct, go and submit it!");
  return 0;
}

_BOOL8 __fastcall check(__int64 a1)
{
  return iven_is_handsome((_BYTE *)a1);
}

_BOOL8 __fastcall iven_is_handsome(_BYTE *a1)
{
  return a1[10] == 112 && a1[13] == 64 && a1[3] == 102 && a1[26] == 114 && a1[20] == 101 && iven_is_c0ol(a1);
}

_BOOL8 __fastcall iven_is_c0ol(_BYTE *a1)
{
  return a1[7] == 48 && a1[16] == 95 && a1[11] == 112 && a1[23] == 101 && a1[30] == 117 && iven_1s_educated(a1);
}

_BOOL8 __fastcall iven_1s_educated(_BYTE *a1)
{
  return *a1 == 119 && a1[6] == 50 && a1[22] == 115 && a1[31] == 110 && a1[12] == 95 && iven_1s_brave(a1);
}

_BOOL8 __fastcall iven_1s_brave(_BYTE *a1)
{
  return a1[15] == 100 && a1[8] == 123 && a1[18] == 51 && a1[28] == 95 && a1[21] == 114 && iven_1s_great(a1);
}

_BOOL8 __fastcall iven_1s_great(_BYTE *a1)
{
  return a1[2] == 116
      && a1[9] == 99
      && a1[32] == 125
      && a1[19] == 118
      && a1[5] == 48
      && a1[14] == 110
      && iven_and_grace(a1);
}

_BOOL8 __fastcall iven_and_grace(_BYTE *a1)
{
  return a1[4] == 50 && a1[17] == 114 && a1[29] == 102 && a1[17] == 114 && a1[24] == 95 && finally_fun(a1);
}

_BOOL8 __fastcall finally_fun(_BYTE *a1)
{
  return a1[1] == 99 && a1[25] == 64 && a1[27] == 101;
}
```


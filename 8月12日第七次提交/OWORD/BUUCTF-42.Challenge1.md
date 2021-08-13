# BUUCTF-42.Challenge1

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char Buffer[128]; // [esp+0h] [ebp-94h] BYREF
  char *Str1; // [esp+80h] [ebp-14h]
  char *Str2; // [esp+84h] [ebp-10h]
  HANDLE v7; // [esp+88h] [ebp-Ch]
  HANDLE hFile; // [esp+8Ch] [ebp-8h]
  DWORD NumberOfBytesWritten; // [esp+90h] [ebp-4h] BYREF

  hFile = GetStdHandle(0xFFFFFFF5);
  v7 = GetStdHandle(0xFFFFFFF6);
  Str2 = "x2dtJEOmyjacxDemx2eczT5cVS9fVUGvWTuZWjuexjRqy24rV29q";
  WriteFile(hFile, "Enter password:\r\n", 0x12u, &NumberOfBytesWritten, 0);
  ReadFile(v7, Buffer, 0x80u, &NumberOfBytesWritten, 0);
  Str1 = (char *)sub_401260(Buffer, NumberOfBytesWritten - 2);
  if ( !strcmp(Str1, Str2) )
    WriteFile(hFile, "Correct!\r\n", 0xBu, &NumberOfBytesWritten, 0);
  else
    WriteFile(hFile, "Wrong password\r\n", 0x11u, &NumberOfBytesWritten, 0);
  return 0;
}
```

1.看程序逻辑，sub_401260为处理函数，处理后与"x2dtJEOmyjacxDemx2eczT5cVS9fVUGvWTuZWjuexjRqy24rV29q"对比

2.

```c
_BYTE *__cdecl sub_401260(int a1, unsigned int a2)
{
  int v3; // [esp+Ch] [ebp-24h]
  int v4; // [esp+10h] [ebp-20h]
  int v5; // [esp+14h] [ebp-1Ch]
  int i; // [esp+1Ch] [ebp-14h]
  unsigned int v7; // [esp+20h] [ebp-10h]
  _BYTE *v8; // [esp+24h] [ebp-Ch]
  int v9; // [esp+28h] [ebp-8h]
  int v10; // [esp+28h] [ebp-8h]
  unsigned int v11; // [esp+2Ch] [ebp-4h]

  v8 = malloc(4 * ((a2 + 2) / 3) + 1);
  if ( !v8 )
    return 0;
  v11 = 0;
  v9 = 0;
  while ( v11 < a2 )
  {
    v5 = *(unsigned __int8 *)(v11 + a1);
    if ( ++v11 >= a2 )
    {
      v4 = 0;
    }
    else
    {
      v4 = *(unsigned __int8 *)(v11 + a1);
      ++v11;
    }
    if ( v11 >= a2 )
    {
      v3 = 0;
    }
    else
    {
      v3 = *(unsigned __int8 *)(v11 + a1);
      ++v11;
    }
    v7 = v3 + (v5 << 16) + (v4 << 8);
    v8[v9] = byte_413000[(v7 >> 18) & 0x3F];
    v10 = v9 + 1;
    v8[v10] = byte_413000[(v7 >> 12) & 0x3F];
    v8[++v10] = byte_413000[(v7 >> 6) & 0x3F];
    v8[++v10] = byte_413000[v3 & 0x3F];
    v9 = v10 + 1;
  }
  for ( i = 0; i < dword_413040[a2 % 3]; ++i )
    v8[4 * ((a2 + 2) / 3) - i - 1] = 61;
  v8[4 * ((a2 + 2) / 3)] = 0;
  return v8;
}
```

其中byte_413000为“ZYXABCDEFGHIJKLMNOPQRSTUVWzyxabcdefghijklmnopqrstuvw0123456789+/”，似乎又是base64的表，解密可以得到“sh00ting_phish_in_a_barrel@flare-on.com”，即为flag
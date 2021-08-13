# BUUCTF-43.crackMe

```c
int wmain()
{
  FILE *v0; // eax
  FILE *v1; // eax
  char v3; // [esp+3h] [ebp-405h]
  char v4; // [esp+4h] [ebp-404h] BYREF
  char v5[255]; // [esp+5h] [ebp-403h] BYREF
  char Format; // [esp+104h] [ebp-304h] BYREF
  char v7[255]; // [esp+105h] [ebp-303h] BYREF
  char password; // [esp+204h] [ebp-204h] BYREF
  char v9[255]; // [esp+205h] [ebp-203h] BYREF
  char user; // [esp+304h] [ebp-104h] BYREF
  char v11[255]; // [esp+305h] [ebp-103h] BYREF

  printf("Come one! Crack Me~~~\n");
  user = 0;
  memset(v11, 0, sizeof(v11));
  password = 0;
  memset(v9, 0, sizeof(v9));
  while ( 1 )
  {
    do
    {
      do
      {
        printf("user(6-16 letters or numbers):");
        scanf("%s", &user);
        v0 = (FILE *)sub_4024BE();
        fflush(v0);
      }
      while ( !(unsigned __int8)checkalnum(&user) );
      printf("password(6-16 letters or numbers):");
      scanf("%s", &password);
      v1 = (FILE *)sub_4024BE();
      fflush(v1);
    }
    while ( !(unsigned __int8)checkalnum(&password) );
    sub_401090(&user);
    Format = 0;
    memset(v7, 0, sizeof(v7));
    v4 = 0;
    memset(v5, 0, sizeof(v5));
    v3 = ((int (__cdecl *)(char *, char *))loc_4011A0)(&Format, &v4);
    if ( (unsigned __int8)sub_401830(&user, &password) )
    {
      if ( v3 )
        break;
    }
    printf(&v4);
  }
  printf(&Format);
  return 0;
}
```

程序完成输入后，首先会调用sub_401090对用户名进行处理，得到一块数据，由于用户名是已知的，因此可以用x32dbg直接获得，数据如下

```c
char data[] = {
	0x5E, 0xBE, 0x4D, 0x43, 0x24, 0xEB, 0x58, 0xE4, 0xC2, 0x5C, 0xBD, 0xC4, 0xAB, 0x39, 0x1D, 0x73, 0x27, 0xD7, 0x88, 0xB8, 0xF6, 0xF9, 0x01, 0xF4, 0x6D, 0xEF, 0x00, 0x15, 0x8C, 0xD5, 0x98, 0x11, 0xA5, 0x25, 0x36, 0x1A, 0x21, 0x47, 0xB4, 0x50, 0xDA, 0xBF, 0x31, 0x32, 0x70, 0xC3, 0x60, 0xB9, 0xDB, 0x81, 0xAD, 0xF2, 0xE0, 0xC5, 0xAC, 0xC0, 0x6F, 0x05, 0xB3, 0x51, 0xE7, 0xA6, 0xAE, 0x6B, 0x8F, 0x7D, 0xCF, 0x7B, 0xA3, 0x56, 0x5A, 0x9A, 0xD4, 0x76, 0x42, 0x9F, 0x53, 0x80, 0xB5, 0x0F, 0xA0, 0xCB, 0x8B, 0xA1, 0x13, 0x2D, 0x85, 0x93, 0xFE, 0xCC, 0x1B, 0xF0, 0x54, 0x61, 0x20, 0x1C, 0xFB, 0x7F, 0x17, 0x64, 0x4A, 0x9D, 0xD0, 0x03, 0xA8, 0x2F, 0x37, 0x30, 0x3D, 0x33, 0xE2, 0x83, 0x8E, 0xD9, 0xB7, 0x18, 0xF7, 0x59, 0xDC, 0x0B, 0x77, 0xAF, 0xEA, 0xB1, 0x10, 0x35, 0x7C, 0x55, 0x4B, 0x74, 0x26, 0xB2, 0x8D, 0x8A, 0x91, 0x97, 0x89, 0xE8, 0xD2, 0x79, 0x3C, 0x44, 0x40, 0x5D, 0xCE, 0x9E, 0x9C, 0x3E, 0x75, 0x29, 0x72, 0x2B, 0xBB, 0x1E, 0x6A, 0x28, 0x2E, 0x62, 0xDF, 0xA2, 0x02, 0x09, 0xBA, 0xC6, 0x3A, 0xB6, 0x4E, 0x7A, 0x71, 0x38, 0x99, 0x65, 0xAA, 0x9B, 0x4C, 0xE6, 0x0C, 0x6C, 0xCD, 0xA7, 0xE3, 0x16, 0xD3, 0x19, 0x49, 0xF5, 0x12, 0xC8, 0x41, 0xDE, 0x06, 0x07, 0x95, 0xD8, 0xFA, 0x66, 0x2A, 0x7E, 0x94, 0x0D, 0x48, 0x90, 0xCA, 0x04, 0xED, 0x67, 0xE5, 0x4F, 0x2C, 0x1F, 0x14, 0xC1, 0xC9, 0xA4, 0xF8, 0x08, 0xE1, 0xB0, 0xEC, 0xE9, 0xA9, 0x34, 0xDD, 0x57, 0x23, 0x87, 0xC7, 0xEE, 0x3B, 0x5B, 0x46, 0x69, 0x84, 0x86, 0x0E, 0x96, 0x78, 0xD1, 0xFD, 0xFF, 0x68, 0xBC, 0x63, 0xF3, 0xD6, 0xFC, 0x82, 0x3F, 0x92, 0x52, 0x6E, 0x5F, 0x22, 0xF1, 0x0A, 0x45
};
```

之后的sub_401830就是判断的主要函数了

```c
bool __cdecl sub_401830(int a1, const char *a2)
{
  int v3; // [esp+18h] [ebp-22Ch]
  int v4; // [esp+1Ch] [ebp-228h]
  int v5; // [esp+28h] [ebp-21Ch]
  unsigned int v6; // [esp+30h] [ebp-214h]
  char v7; // [esp+36h] [ebp-20Eh]
  char v8; // [esp+37h] [ebp-20Dh]
  char v9; // [esp+38h] [ebp-20Ch]
  unsigned __int8 v10; // [esp+39h] [ebp-20Bh]
  unsigned __int8 v11; // [esp+3Ah] [ebp-20Ah]
  char v12; // [esp+3Bh] [ebp-209h]
  int v13; // [esp+3Ch] [ebp-208h] BYREF
  char v14; // [esp+40h] [ebp-204h] BYREF
  char v15[255]; // [esp+41h] [ebp-203h] BYREF
  char v16; // [esp+140h] [ebp-104h] BYREF
  char v17[255]; // [esp+141h] [ebp-103h] BYREF

  v4 = 0;
  v5 = 0;
  v11 = 0;
  v10 = 0;
  v16 = 0;
  memset(v17, 0, sizeof(v17));
  v14 = 0;
  memset(v15, 0, sizeof(v15));
  v9 = 0;
  v6 = 0;
  v3 = 0;
  while ( v6 < strlen(a2) )
  {
    if ( isdigit(a2[v6]) )
    {
      v8 = a2[v6] - 48;
    }
    else if ( isxdigit(a2[v6]) )
    {
      if ( *((_DWORD *)NtCurrentPeb()->SubSystemData + 3) != 2 )
        a2[v6] = 34;
      v8 = (a2[v6] | 0x20) - 87;
    }
    else
    {
      v8 = ((a2[v6] | 0x20) - 97) % 6 + 10;
    }
    __rdtsc();
    __rdtsc();
    v9 = v8 + 16 * v9;
    if ( !((int)(v6 + 1) % 2) )
    {
      *(&v14 + v3++) = v9;
      v9 = 0;
    }
    ++v6;
  }
  while ( v5 < 8 )
  {
    v10 += byte_416050[++v11];
    v12 = byte_416050[v11];
    v7 = byte_416050[v10];
    byte_416050[v10] = v12;
    byte_416050[v11] = v7;
    if ( ((int)NtCurrentPeb()->UnicodeCaseTableData & 0x70) != 0 )
      v12 = v10 + v11;
    *(&v16 + v5) = byte_416050[(unsigned __int8)(v7 + v12)] ^ *(&v14 + v4);
    if ( (unsigned __int8)*(_DWORD *)&NtCurrentPeb()->BeingDebugged )
    {
      v10 = -83;
      v11 = 43;
    }
    sub_401710(&v16, a1, v5++);
    v4 = v5;
    if ( v5 >= (unsigned int)(&v14 + strlen(&v14) + 1 - v15) )
      v4 = 0;
  }
  v13 = 0;
  sub_401470(&v16, &v13);
  return v13 == 0xAB94;
}
```

该函数中第一个while循环为将每两个passwd字符按16进制组成一个整形数字放在v14，第二个while循环为根据数字数组生成v16，最终再将v16传入sub_401470，返回值应为0xAB94

先看sub_401470

```c
_DWORD *__usercall sub_401470@<eax>(int a1@<ebx>, _BYTE *a2, _DWORD *a3)
{
  char v5; // al
  _DWORD *result; // eax

  if ( *a2 != 0x64 )
    *a3 ^= 3u;
  else
    *a3 |= 4u;
  if ( a2[1] != 0x62 )
  {
    *a3 &= 0x61u;
    _EAX = (_DWORD *)*a3;
  }
  else
  {
    _EAX = a3;
    *a3 |= 0x14u;
  }
  __asm { aam }
  if ( a2[2] != 0x61 )
    *a3 &= 0xAu;
  else
    *a3 |= 0x84u;
  if ( a2[3] != 0x70 )
    *a3 >>= 7;
  else
    *a3 |= 0x114u;
  if ( a2[4] != 0x70 )
    *a3 *= 2;
  else
    *a3 |= 0x380u;
  if ( *((_DWORD *)NtCurrentPeb()->SubSystemData + 3) != 2 )
  {
    if ( a2[5] != 0x66 )
      *a3 |= 0x21u;
    else
      *a3 |= 0x2DCu;
  }
  if ( a2[5] != 0x73 )
  {
    v5 = (char)a3;
    *a3 ^= 0x1ADu;
  }
  else
  {
    *a3 |= 0xA04u;
    v5 = (char)a3;
  }
  _AL = v5 - (~(a1 >> 5) - 1);
  __asm { daa }
  if ( a2[6] != 0x65 )
    *a3 |= 0x4Au;
  else
    *a3 |= 0x2310u;
  if ( a2[7] != 0x63 )
  {
    *a3 &= 0x3A3u;
    result = (_DWORD *)*a3;
  }
  else
  {
    result = a3;
    *a3 |= 0x8A10u;
  }
  return result;
}
```

可以得出v16应为"dbappsec"，再由*(&v16 + v5) = byte_416050[(unsigned __int8)(v7 + v12)] ^ *(&v14 + v4);可以计算出v14的值应为0x4e, 0xb5, 0xf3, 0x99, 0x23, 0x91, 0xa1, 0xae，则passwd为4eb5f3992391a1ae,flag为其md5值，则为：flag{d2be2981b84f2a905669995873d6a36c}


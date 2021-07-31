# BUUCTF-17.pyre

记录下pyc程序的逆向流程

1.用Easy Python Decompiler生成.pyc_dis文件，这就是python源码，用文本编辑器打开查看，代码如下：

```python
# Embedded file name: encode.py
print 'Welcome to Re World!'
print 'Your input1 is your flag~'
l = len(input1)
for i in range(l):
    num = ((input1[i] + i) % 128 + 128) % 128
    code += num

for i in range(l - 1):
    code[i] = code[i] ^ code[i + 1]

print code
code = ['\x1f',
 '\x12',
 '\x1d',
 '(',
 '0',
 '4',
 '\x01',
 '\x06',
 '\x14',
 '4',
 ',',
 '\x1b',
 'U',
 '?',
 'o',
 '6',
 '*',
 ':',
 '\x01',
 'D',
 ';',
 '%',
 '\x13']
```

2.分析代码得出，程序先令输入的每一个字符 = ((input1[i] + i) % 128 + 128) % 128，然后再每位与后一位相与，得到列表code，我们只需倒序将列表的每一项的后一项相与，然后再用爆破求出flag，求解代码如下：

```c
#include <stdio.h>
char table[] = {'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', '{', '}', '_', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '!', '@', '#', '$', '%', '^', '&', '*', '(', ')', '-', '+', '=', '\\', '/'};
int main(void)
{
	char code[] = {'\x1f', '\x12', '\x1d',  '(', '0', '4', '\x01', '\x06', '\x14', '4', ',', '\x1b', 'U', '?', 'o', '6', '*', ':', '\x01', 'D', ';', '%', '\x13'};
	char input[24] = {0};
	int l = 23, tl = 80;
	for(int i = l - 1; i >= 0; --i)
	{
		code[i] ^= code[i + 1];
	}
	for(int i = 0; i < l; ++i)
	{
		for(int j = 0; j < tl; ++j)
		{
			int num = ((table[j] + i) % 128 + 128) % 128;
			if(num == code[i])
			{
				input[i] = table[j];
				break;
			}
		}
	}
	printf("%s", input);
	return 0;
}
```

flag为：GWHT{Just_Re_1s_Ha66y!}
# Bugku-20.MountainClimbing

1.该程序的代码片段也是在运行时即使解密的，因此静态分析时需要dump出来，主要的代码在411B70处，以下为ida生成的伪代码：

![20-1-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/20.MountainClimbing/img/20-1-1.png)

![20-1-2](https://github.com/OWORD/ctfimg/raw/main/Bugku/20.MountainClimbing/img/20-1-2.png)

2.由代码可以看出，我们的输入长度必须为19，经sub_41114F处理后，所有字符只能为"L"或“R”，且二者的加点数不同，需要我们构造出一个加点数最多的LR字符串，另外，sub_41114F这个函数我没有跟入分析，只是测试了下，发现奇数位字符不变，偶数位字符H映射到L，V映射到R。

3.理解原理后就可以尝试爆破LR字符串了，我们先把0x41A138开头，长度为0x2000的内存数据dump出来，然后读入我们的程序，来模拟程序的计算点数的过程，然后对每位都尝试“L”和“R”，然后找到最大点数对应的字符串，总共可以构造出2^19种字符串，即便如此，交给计算机处理也不过几秒钟而已，具体代码如下：

```c
#include <iostream>
using namespace std;

unsigned int* mem;
const char mempath[] = "E:/ctf/Bugku/20.MountainClimbing/MEM_0041A138_00002000.mem";//dump文件路径

void LoadMem()
{
    FILE* fp = fopen(mempath, "rb");
    fseek(fp, 0, SEEK_END);
    size_t size = ftell(fp);
    fseek(fp, 0, SEEK_SET);
    mem = (unsigned int*)malloc(size);
    fread(mem, 1, size, fp);
    fclose(fp);
}

unsigned int test(char* str)
{
    unsigned int v2 = 0;
    unsigned int v4 = 1;
    unsigned int v6 = 1;
    unsigned int point = mem[101];
    while(v2 < 19)
    {
        if(str[v2] == 76){
            ++v6;
            point += mem[100 * v6 + v4];
        }else{
            ++v6;
            ++v4;
            point += mem[100 * v6 + v4];
        }
        ++v2;
    }
    return point;
}

int main(void)
{
    LoadMem();
    char str[20] = {0};
    char maxStr[20] = {0};
    unsigned int max = 0;
    unsigned int temp = 0;
    unsigned int andMask[19] = {1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192, 16384, 32768, 65536, 131072, 262144};
    for(int i = 0; i < 524288; i++)
    {
        for(int j = 0; j < 19; j++)
        {
            if(andMask[j] & i)
                str[j] = 'L';
            else
                str[j] = 'R';
        }
        temp = test(str);
        if(temp > max)
        {
            max = temp;
            memcpy(maxStr, str, 19);
        }
    }
    
    printf("%u:%s", max, maxStr);
    return 0;
}
```

运行得到最大点数为444740，对应的字符串为”RRRRRLLRRRLRLRRRLRL“，转换后为RVRVRHLVRVLVLVRVLVL，则flag为：zsctf{RVRVRHLVRVLVLVRVLVL}
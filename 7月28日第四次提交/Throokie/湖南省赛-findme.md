# 1.png

得到的文件

![image-20210731162044038](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731162044038.png)

发现1.png很明显与其他不同，010editor打开看下。发现报crc错误

crc是根据[12:29]位计算得到的。这些位置上是IDCH和IHDR数据块。

### 代码

```python
import zlib
import struct

filename = r'C:\Users\。\Desktop\Findme\1.png'
with open(filename, 'rb') as f:
    all_b = f.read()
    crc32key = int(all_b[29:33].hex(), 16)
    data = bytearray(all_b[12:29])
    n = 4095  # 理论上0xffffffff,但考虑到屏幕实际/cpu，0x0fff就差不多了
    for w in range(n):  # 高和宽一起爆破
        width = bytearray(struct.pack('>i', w))  # q为8字节，i为4字节，h为2字节
        for h in range(n):
            height = bytearray(struct.pack('>i', h))
            for x in range(4):
                data[x + 4] = width[x]
                data[x + 8] = height[x]
            crc32result = zlib.crc32(data)
            if crc32result == crc32key:
                # 2021.7.20，有时候显示的宽高依然看不出具体的值，干脆输出data部分
                print(data.hex())
                print("宽为：", end="")
                print(width)
                print("高为：", end="")
                print(height)
                exit(0)
```

![image-20210731162732165](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731162732165.png)



改动后

![image-20210731163122030](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731163122030.png)

发现还是不正常。检查chunk发现chunk[2]、chunk[3]有问题

![image-20210731163403764](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731163403764.png)

追踪到有问题的chunk的对应标识位置。修改成49 44 41 54。表示IDAT

![image-20210731163812860](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731163812860.png)

修改成功。得到完整图片

![image-20210731164309638](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731164309638.png)

放进Stegsolve查看

![image-20210731164332049](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731164332049.png)

扫描二维码

![image-20210731164441076](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731164441076.png)

```
ZmxhZ3s0X3
```

# 2.png

![image-20210731165431541](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731165431541.png)

图片结束了，后面还有内容，看到7z字样，是个压缩包。提取出来，发现010editor无法识别成7zip格式。

![image-20210731170431632](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731170431632.png)

![image-20210731170438575](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731170438575.png)

检查文件头，发现前面两个字节是7zip的，后面两个字节是zip的

```
ZIP Archive (zip)，文件头：50 4B 03 04
7z文件头：37 7A BC AF 27 1C
```

### 改文件头

改全部7z文件头。成功打开压缩包。

![image-20210731170815011](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731170815011.png)

发现很多都是看起来一样大小(1kb)的文件。这时候进行排序，得到了不同的文件

![image-20210731170948932](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731170948932.png)

打开该文件，得到第二段base64代码

![image-20210731171019667](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731171019667.png)

```
1RVcmVfc
```

# 3.png

打开，又是crc error，而且还是每段数据都error

![image-20210731171224475](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731171224475.png)

但是图片是确定没问题的。不然显示不了这么正常。

这里就要看脑洞了，看crc数据，发现数据只占用一个字节（十进制255）。说明是ascii码

手动解码得到第三块base64解码

```
3RlZ30=
```

# 4.png

![image-20210731171658310](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731171658310.png)

没有错误。那图片正常。看下图片EXIF信息

[在线网址](https://exif.tuchong.com/view/11236588/)

![image-20210731171803818](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731171803818.png)

```
cExlX1BsY
```



# 5.png

![image-20210731171856587](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731171856587.png)

结尾藏有数据。直接追踪过去

![image-20210731171914923](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731171914923.png)

最后一段base64编码得到了

```
Yzcllfc0lN
```



# flag

将有等于号的base64放最后，其他靠感觉排列。

或者写脚本尝试

![image-20210731172717437](%E6%B9%96%E5%8D%97%E7%9C%81%E8%B5%9B-findme.assets/image-20210731172717437.png)
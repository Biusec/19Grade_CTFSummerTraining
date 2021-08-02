# BUUCTF-27.相册

1.首先用jadx打开，发现有一个so库，还有一些Base64相关的类，推测本题会用到base64解密，先用ida打开so库，搜索字符串，发现3个类似base64码的字符串，其中“MTgyMTg0NjUxMjVAMTYzLmNvbQ==”解码后是一个邮箱，则flag为改邮箱
# Bugku-18.SpecialBase64

1.用ida打开，定位到main函数，f5得到伪代码：

![18-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/18.SpecialBase64/img/18-1.png)

2.由以上代码看出base64Encode函数对用户输入进行加密，并与“mTyqm7wjODkrNLcWl0eqO8K8gc1BPk1GNLgUpI==”进行比较，转到base64Encode函数，发现其与正常的base64不同之处就在于映射字符不同，该base64映射字符为“AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz0987654321/+”，这个网站可以根据自定义的映射字符进行base64加解密：[在线自定义base64编解码、在线二进制转可打印字符、在线base2、base4、base8、base16、base32、base64--查错网 (chacuo.net)](http://web.chacuo.net/netbasex)，最终解得flag：flag{Special_Base64_By_Lich}




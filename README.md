# FastReqTest

#### 介绍
通过http原始包进行快速测试,适用场景：  
1. 从某处得到一个漏洞的payload原始包，不用开burpsuite随手就验证一下。
2. 从某处得到一个漏洞的payload原始包，批量对地址进行验证。
3. 小菜鸡玩awd比赛时，不用写脚本了，通过自己服务器上找出漏洞，然后一键全场种马，一键全场拿flag。。。

#### 软件架构
采用golang编写，所以全系统支持，包括安卓手机系统。


#### 使用说明

1.  没有任何参数时，程序默认加载当前目录的"req.txt"这个文件，并返回请求结果的状态码。"req.txt"文件中应保存原始数据包。
2.  参数说明：参数分为运行参数和输出参数。  
 
运行参数：  
|参数名称|参数说明|默认值|
|-------|-------|------|
|-h|指定主机，不设置从数据包中获取主机地址|无|
|-t|设置主机列表文件路径|无|
|-r|设置数据包文件路径|当前目录下的req.txt|
|-T|设置请求超时时间|5秒|
|-P|设置代理地址|无|
|-p|设置多线程数量|默认单线程|

输出参数：  
 
|参数名称|参数说明|
|-------|-------|
|-oS|输出返回状态码，如果所有输出参数均未配置则自动输出状态码|
|-oL|输出返回正文长度|
|-oT|输出正文内容。尽量别用，输出内容太多。如确实需正文完整内容，建议配置代理到burpsuite查看|
|-oH|输出指定header内容，如果设置为all则以golang map格式输出所有header内容(内容较长，不建议输出！)|
|-oR|输出raw格式返回包信息到文件|
|-oG|设置正则匹配内容|
|-oGF|配合正则匹配内容，成功返回true，否则返回false|
|-oGS|配合正则匹配内容，成功返回匹配到部分的内容|


3.  在-h或者-t的文件中，设置的地址必须包含http:// 或者https:// 的头，结尾部分可以包含路径，也可以没有。包含路径时会使用路径替换数据包中请求的路径，不包含路径时仅替换数据包的请求主机地址。

4. 例子1：  
```
PS D:\code\golang\FastReqTest> .\FastReqTest.exe -r req.txt -h https://www.baidu.com -P http://127.0.0.1:8080 -T 10
https://www.baidu.com   200
任务执行结束，共耗时： 6.3194358s
PS D:\code\golang\FastReqTest> 
```
5. 例子2：  
```
PS D:\code\golang\FastReqTest> .\FastReqTest.exe -r req.txt -h https://www.baidu.com -P http://127.0.0.1:8080 -T 10 -oS -oH Set-Cookie    
https://www.baidu.com   200     [BAIDUID=C2DE8D032D6EF3BB0D0B7799F2D1CB36:FG=1; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com BIDUPSID=C2DE8D032D6EF3BB0D0B7799F2D1CB36; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com PSTM=1612273802; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com BAIDUID=C2DE8D032D6EF3BBC265D708889DF6D1:FG=1; max-age=31536000; expires=Wed, 02-Feb-22 13:50:02 GMT; domain=.baidu.com; path=/; version=1; comment=bd BDSVRTM=0; path=/ BD_HOME=1; path=/ H_PS_PSSID=33425_33514_33442_33256_33344_31253_33459_26350_33264; path=/; domain=.baidu.com]       
任务执行结束，共耗时： 340.5941ms
PS D:\code\golang\FastReqTest>
```
6. 例子3:
```
PS D:\code\golang\FastReqTest> .\FastReqTest.exe -r req.txt -h https://www.baidu.com -P http://127.0.0.1:8080 -T 10 -oS -oG \<title\>.*?\<\/title\> -oGF
https://www.baidu.com   200     true
任务执行结束，共耗时： 320.5948ms
PS D:\code\golang\FastReqTest> 
```
7. 例子4：  
```
PS D:\code\golang\FastReqTest> .\FastReqTest.exe -r req.txt -h https://www.baidu.com -P http://127.0.0.1:8080 -T 10 -oS -oG \<title\>.*?\<\/title\> -oGS
https://www.baidu.com   200     [<title>百度一下，你就知道</title>]
任务执行结束，共耗时： 322.7107ms
PS D:\code\golang\FastReqTest> 
```
以上所有例子均使用https://www.baidu.com 的原始包进行测试。

#### 参与贡献

1. xiaoguaiii



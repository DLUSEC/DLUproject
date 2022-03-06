# 信息收集

## 收集方向

- DNS：

  - 确定企业网站运行规模；
  - 可以从DNS中收集子域名、IP等；
  - 控制网站解析

- 子域名：

  - 确定企业网站运行数量，从而进行下一步(安全评估)准备；
  - 获得不同子域名所映射的IP，从而获得不同C段；
  - 寻找更大的安全脆弱点和面

- C段：

  ​	什么是C段

  在IP地址的4段号码中，前3段号码为网络号码，剩下的1段号码为本地计算机的号码**192.168.1.5/24**

  - 确定C段存活主机数量；
  - 确定C段中主机的端口，服务,操作系统等

- 邮箱：

  - 通过分析邮箱格式和后缀,可以得知邮箱命名规律和邮箱服务器
  - 为爆破登录表单收集数据，可形成字典:
  - 发送钓鱼邮件，执行高级APT控制

- 指纹

  - WEB指纹
    - 获取运行的脚本语言，开发框架，CMS,寻找脆弱点(漏洞)
      如: .action 一般可以确定为Struts2
      如: Powered by **
  - 中间件指纹
    - 获取中间件使用的产品和版本
      通过产品和版本查询是否有漏洞存在，如: struts2反序列化，iis文件解析
  - 系统指纹
    - 获取操作系统使用的产品和版本
      可以在以后渗透中提供渗透基准
      如:大小写，shell部署方式

- 社工库

  - 寻找指定目标的已经泄露的数据
    如:邮箱，获取到企业内部人员已经泄露的密码，可以在撞库，爆破中使用
    如:姓名，手机号，找回密码，重置信息

- 钓鱼攻击

  - 邮件、 链接、办公文件

  - 构造鱼 叉攻击和水坑攻击

  - 绕过边界防御设备

  - 从内部瓦解防御网络， 直接反弹shell



## 初步工作

```
nslookup xxx.com
```

```shell
nslookup www.baidu.com

Server:		192.168.31.1
Address:	192.168.31.1#53

Non-authoritative answer:
www.baidu.com	canonical name = www.a.shifen.com.
Name:	www.a.shifen.com
Address: 110.242.68.5
Name:	www.a.shifen.com
Address: 110.242.68.3
```

### dig命令：https://www.jianshu.com/p/f6ef04bf6af2

dig是一个在类Unix命令行模式下查询DNS包括NS记录，A记录，MX记录等相关信息的工具；

最常用的查询是A记录，TXT（文本注释），MX记录，NS记录，或者任意综合查询。

#### 命令选项

```
 -b address设置所要询问地址的源IP地址。这必须是主机网络接口上的某一合法的地址
 
 -c class缺省查询类，由选项-c重设。class可以是任何合法类，比如查询Hesiod记录的HS类或查询CHAOSNET记录的CH类；
 
 -f filename使dig在批处理模式下运行，通过从文件filename读取一系列搜索请求加以处理。文件包含许多查询，每行一个。文件中的每一项都应该和使用命令行接口对dig查询相同的方法来组织；-f filename使dig在批处理模式下运行，通过从文件filename读取一系列搜索请求加以处理。文件包含许多查询，每行一个。文件中的每一项都应该和使用命令行接口对dig查询相同的方法来组织；
 
 -h显示一个简短的命令行参数和选项摘要；-h显示一个简短的命令行参数和选项摘要；
 
 -k filename要签署由dig发送的DNS查询以及对他们使用事物签名（TSIG）的响应，用选项-K指定TSIG密钥文件；-k filename要签署由dig发送的DNS查询以及对他们使用事物签名（TSIG）的响应，用选项-K指定TSIG密钥文件；
 
 -n
 -p port如果需要查询一个非标准的端口号，则使用选项-p。port是dig将发送其查询的端口号，而不是标准的DNS端口号53。该选项可用于测试已在非标准端口号上配置成侦听查询的域名服务器；-p port如果需要查询一个非标准的端口号，则使用选项-p。port是dig将发送其查询的端口号，而不是标准的DNS端口号53。该选项可用于测试已在非标准端口号上配置成侦听查询的域名服务器；
 
 -t type设置查询的类型为type。可以是BIND9支持的任意有效查询类型。缺省查询类型是A，除非提供-x选项来指示一个逆向查询。通过指定AXFR的type可以请求一个区域传输。当需要增量区域传输（IXFR）时，type设置为ixfr=N，增量区域传输将包含自从区域的SOA记录中的序列号改为N之后对区域所做的更改；-t type设置查询的类型为type。可以是BIND9支持的任意有效查询类型。缺省查询类型是A，除非提供-x选项来指示一个逆向查询。通过指定AXFR的type可以请求一个区域传输。当需要增量区域传输（IXFR）时，type设置为ixfr=N，增量区域传输将包含自从区域的SOA记录中的序列号改为N之后对区域所做的更改；
 
 -x addr 逆向查询（将地址映射到名称）；-x addr 逆向查询（将地址映射到名称）；
 
 -y name key指定TSIG秘钥；-y name key指定TSIG秘钥；
```

- DNS **A**记录：（此处一定是域而不是主机，如我公司为xinpindao.com)

  ```sh
  nslookup www.baidu.com
  dig baidu.com
  ```

- DNS **MX**记录的列表： **MX 记录是邮件交换记录，它指向一个邮件服务器**

  用于电子邮件系统发邮件时根 据收信人的地址后缀来定位邮件服务器

  ```sh
  dig -t MX 163.com
  ```

- DNS **NS**记录,**NS记录是域名服务器记录，用来指定该域名由哪个DNS服务器来进行解析。**

  ```sh
  dig -t NS www.xxx.com
  ```

- DNS TXT记录

  **TXT记录，一般指某个主机名或域名的说明**

  ```sh
  nslookup -qt=txt www.baidu.com
  ```

- DNS CNAME记录:**CNAME记录可以将注册的不同域名都转到一个域名记录上，由整个域名记录统一解析管理**

  ```sh
  dig -t CNAME 163.com
  ```



### Whois查询域名的IP以及所有者等信息的传输协议

```
whois baidu.com
```

#### 域名信息在线查询：

- whois .iana.org
- www.arin.net
- centralops.net/co/
- www.17ce.com
- whois.cloud.tencent.com
- www.chinaz.com

### 子域名收集方法

> - 爆破
> - 搜索引擎
> - 域传送
> - 在线网站

#### 爆破子域名:oneforall

#### 搜索引擎

通过搜索引擎获取以及爬取的子域名:**theharvester**

#### 域传送

DNS区域传送指的是一台备用服务器使用来自主服务器的数据刷新自己的域（zone）数据库。为运行中的DNS服务器提供了一定的冗余度，目的是为了防止主的域名服务器因意外故障变得不可用时影响整个域名的解析
DNS区域传送操作只在网络中真的有备用域名DNS服务器时才会有必要用到，但许多DNS服务器却被错误地配置成只要有client发出请求，就会向对方提供一个zone数据库的详细信息，所以说允许不受信任的因特网用户执行DNS区域传送

##### 危害

黑客可以快速判断出某个特定zone的所有主机，收集域信息，选择攻击目标，找出未使用的IP地址，黑客可以绕过基于网络的访问控制。

#### 在线网站

```http
www.virustotal.com
dnsdumpster.com
tool.chinaz.com 
```

### C段扫描

- nmap
- k8/挖掘鸡

- masscan
- bingc
- 浏览器查

### web目录检索

- dirmap 

- DirBuster
- 御剑

### 利用浏览器搜索关键词

- inurl:搜索网页上包含的url
- intext:只搜索网页部分中包含的文字
- site:现在搜索域名的范围
- filetype:搜索文件后缀或扩展名
- intitle:限制搜索的网页标题
- allintitle:所有关键字构成的标题
- link:可以得到一个所有包含了某个指定url的页面列表;例如：link:www.google.com就可以得到所有连接到google的页面

### 指纹识别

Wappalyzer

#### 防火墙指纹识别

wafw00f

### 情报分析

- maltego （https://blog.csdn.net/smli_ng/article/details/105943189）





## 工具

DNSDB:https://www.dnsdb.io/zh-cn/

Whois查询： https://whois.chinaz.com/

某社工库：https://www.instantcheckmate.com/

shodan:https://www.shodan.io/

fofa:https://fofa.so/

查源码：https://searchcode.com/

华为安全平台：https://isecurity.huawei.com/sec/web/urlClassification.do

钟馗之眼：https://www.zoomeye.org/

网盘搜索：http://www.pansou.com/

微步：https://x.threatbook.cn/

**伪造邮件等各种邮件工具：http://tool.chacuo.net/mailanonymous**

子域名：

- dns域传送漏洞:dnsenum baidu.com
- SSL状态检测： https://myssl.com/ssl.html
- ICP备案查询：http://icp.chinaz.com/
- crossdomain.xml文件配置不当利用手法:https://cloud.tencent.com/developer/article/1035542
- 暴力枚举：subDomainBrute/sublist3r
- DNS历史解析：https://ww.dnsdb.io
- 利用google搜索c段




---
layout: post
title: 浅谈信息收集
tags: [information_collection,google,kali]
lastUpdate: 2020-02-10
author: 404player
---        
  
信息收集是进行渗透测试最开始也是最重要的一个环节，顾名思义，信息收集就是通过各种手段去收集渗透所需要的信息，广义的信息收集包括技术手段和社工，出于篇幅限制，本文所讨论的都是用技术手段进行的信息收集。  
  
那么信息收集主要收集什么？该如何收集呢？    
<!--more-->   
- - - 
  
## 1.whois信息    
> whois（读作“Who is”，非缩写）是用来查询域名的IP以及所有者等信息的传输协议  
  
简单来说，whois信息其实就是域名的详细信息，包括域名的注册商、注册人、邮件、DNS解析服务器、注册人联系电话都是值得关注的，我们可以通过这些信息去进一步地进行信息收集。那么收集whois信息的方法有什么呢？  
 

- 通过网站进行查询  
    - 站长之家：http://whois.chinaz.com/
    - 爱站：https://whois.aizhan.com/   
    - 国外的 who.is ： https://who.is/s    
- 可以通过kali系统的whois命令  
    - whois domain/ip  
- 通过查询备案信息进行whois查询  
    - ICP备案查询网：http://www.beianbeian.com/   
    - 天眼查：https://www.tianyancha.com/   
      
值得注意的一点是，这里所说的whois查询针对的都是国内的服务器，国外的服务器一般是不需要备案的。    
- - - 
  
## 2.子域名查询  
  
> 子域名（或子域；英语：Subdomain）是在域名系统等级中，属于更高一层域的域。比如，mail.example.com和calendar.example.com是example.com的两个子域，而example.com则是顶级域.com的子域。    
  
### 挖掘子域名的重要性：  
子域名是某个主域的二级域名或者多级域名，在防御措施严密的情况下无法直接拿下主域，那么就可以采取迂回战术拿下子域名，然后无限拿下主域。  
  
### 获取子域名的方法：    
1.可以使用子域名挖掘工具进行挖掘，这里推荐使用kali自带的域名挖掘工具maltego。  
![maltego](https://img-blog.csdnimg.cn/20200210222851517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70)  
  
上图是我用maltego收集到的`51cto.com`的子域名  
  
2.可以利用搜索引擎进行挖掘，`Google hacking`一直以来都是比较流行的信息收集的方法，当然其他搜索引擎也多少兼容一点`Google hacking`的语法，只是没有那么全面。  
<img src="https://img-blog.csdnimg.cn/20200210222918402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70" width = "1000" height = "500">  
    
就如上图所示，查找某个主域的子域名的语法为：`site:domain`    
  
3.可以通过第三方网站进行子域名的查询。  
    
- 站长之家：https://tool.chinaz.com/subdomain/    
- 子域名收集：https://phpinfo.me/domain/  
  
4.GitHub上也有很多用爆破枚举方法写成的子域名收集工具，并有很清晰的  `readme.md`说明文档，可以尝试搜索并使用。我曾经试过一种叫`dnsburte`的工具，是一款基于python环境使用的工具，效果不错。  
  
5.基于SSL证书查询  
查找一个域名证书的最简单方法是使用搜索引擎来收集计算机的CT日志，域名证书查询的网站中大多都会包含有子域名信息，可以通过证书查询的方法去获取子域名。  
    
下面给出几个用于证书查询的网站：  
- https://crt.sh/
- https://censys.io/(云悉)  
  
> 刷洞思路：针对某个具体SRC进行挖洞，可以搜集对应的子域名，然后批量探测某些具体漏洞(一般是根据某些具体漏洞，通过url采集获取目标，编写探测脚本批量探测)，从而快速容易地发现一些漏洞。    
   
- - - 
  
## 3.Web站点信息的收集   
  
1.CMS指纹识别  
CMS，全称为content management system，中文翻译过来就是内容管理系统，顾名思义，就是用于网站内容管理的一套系统。很多网站都是基于现有的CMS搭建起来的，各种CMS都具有独特的结构命名规则和特定的文件内容，因此可以利用这些内容来获取CMS站点和具体的CMS版本。  
  
常见的CMS有：WordPress、Joomla!、Drupal、dedecms，Discuz、Phpcms  
  
那么使用什么方法才能识别到一个网站所使用的CMS呢？  
  
- 可使用第三方网站进行搜索  
    - http://whatweb.bugscaner.com/look  
    - http://www.yunsee.cn/finger.html  
- 也可使用本地工具  
    - kali中可使用命令行`whatweb domain（域名）`查询对应域名的CMS  
    - 御剑指纹识别  
    - 大禹指纹识别（可以在github）上找到  
      
      
针对查询到的CMS，可利用`wooyun.x10sec.org`在线网站查询CMS漏洞信息，根据CMS漏洞信息对网站安全性进行进一步的探测。  
  
2.敏感目录信息  
   
    针对目标web目录结构和敏感隐藏文件探测是非常重要的，很有可能探测出网站的后台页面、上传页面、数据库文件甚至是网页源代码及文件压缩包等。    
      
    
      
通常我们说的敏感文件、敏感目录一般分为以下几种：  
```
 1）Git

（2）hg/Mercurial

（3）svn/Subversion

（4）bzr/Bazaar

（5）Cvs

（6）WEB-INF泄露

（7）备份文件泄露、配置文件泄露
```  
  
挖掘敏感文件、敏感目录一般靠脚本挖掘，下面介绍以下我所用过的工具：  
1.御剑后台探测       
2.wwwscan命令行  
3.爬虫(AWVS,burpsuite等)  
4.dirb命令行工具(kali自带，可通过`dirb -h`命令查询工具用法)  
5.dirburte扫描工具  
6.Google hacking  
  
因为在信息搜集的过程中经常要用到Google hacking，所以下面来介绍一些其用法：  
  
```  
site:搜寻子域名  
inurl:指定URL是否存在某些关键字                   例：inurl:php?id=  
intext:指定网页中是否存在某些关键字  
filetype:指定搜索文件类型                         例：filetype:txt  
intitle:指定网页标题是否存在某些关键字  
link:指定网页链接                                例：link:baidu.com指定与百度做了外链的站点  
info:搜索网页信息
```  
  
此外，我们还可以通过HTTP响应收集Server信息：通过目标响应报文中Server头和X-Powered-By头会暴露目标服务器和使用的编程语言信息。而要获得这些信息，我们可以考虑使用浏览器的开发者工具审计或者burpsuite等抓包工具去获取，搜索到信息后，可使用kali自带的searchspolit进行指定编程语言与框架的漏洞搜索。      
  
> 敏感信息收集的重要性:针对某些安全做的很好的目标，直接通过技术手段层面无法完成渗透。可以利用搜索引擎搜索目标暴露在互联网上的关联信息。  

  

  
3.端口信息  
如果把IP地址或者域名比喻成一间房子，端口则是进出这间房间的门口或窗户，一个IP地址可以有65536个端口，每个端口代表一个服务，在Windows命令行中使用`netstat -anbo`去显示本地开放端口(可以使用`netstat -h`去看看anbo分别代表什么意思)  
  
端口信息的收集：  
  
- 可以使用工具进行收集（工具原理：使用TCP或UDP等协议向目标端发送指定标志位等的数据包，等待目标返回数据包，以此来判断端口状态）  
    - 使用namp探测  
        - 语法：namp -A -v -T4  目标域名  
          
    - 使用masscan探测 
        - 语法：masscan -p80 目标  --rate=10000  
          
使用工具固然方便，但是因为其发送数据包的工作原理，在使用工具时无可避免地会留下痕迹，使用在线网站探测就可以解决这个问题（反正就算留下痕迹也不是自己的）  
  
- 使用在线网站探测  
    - http://tool.chinaz.com/port    
      
        
不同的端口有不同的漏洞，因此针对不同的端口有不同的攻击方式：  
下面写出几个针对远程连接服务的端口  
  
|端口号|端口服务|攻击方式|
|-------|----|-------|
|  22   |SSH远程连接|弱口令爆破，SSH隧道及内网代理转发，文件传输等|
|23 |Telnet|嗅探，弱口令爆破 |
|3389     |RDP远程桌面|弱口令爆破，SHIFT后门，放大镜，输入法漏洞|
|5900    |VNC远程连接 |弱口令爆破|
|5632   |PCAnywhere远程连接|	嗅探，代码执行|    
  
```
端口攻击的防御方法：  
1.关闭不必要的端口  
2.对重要业务的服务端口设置防火墙  
3.经常更新软件，打补丁    
```     
  
4.收集域名真实IP  
  
很多时候，很多网站会开启CDN加速。什么叫CDN加速呢？就是用户在网站中无需进行交互操作时，往往时根据所在地来确定访问一个高速缓存服务器，只有在需要交互时才会请求真实的服务器。这给我们收集IP设置了一层阻碍。  

在搜集IP时我们要先确定网站是否存在CDN，因为CDN费用不便宜，一般只存在一些大型网站，我们可以在Windows终端用ping去判断：  `语法：ping doamin`  

![CDN](https://img-blog.csdnimg.cn/20200211115321806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70)    
  
上图在ping百合网的域名时出现了`cdntip.com`,证明网站存在CDN加速  
  
也可以通过设置代理或者利用在线ping网站（`http://ping.chinaz.com`）来使用不同地区的ping服务器来测试目标，使用ping网站检测会出现很多个对应此域名的IP，在网址栏直接通过IP访问，如果是真实的IP，标签栏中会显示该网站的图标，反之则会出现找不到服务器的提示。  
  
绕过CDN搜寻真实IP：  
1.通过让服务器给你发邮件(看邮箱头源 ip )找真实ip  
2.通过 zmpap 全网爆破查询真实ip  
3.查询域名解析的记录    
- http://viewdns.info/
  
- - -   
  
## 4.旁站，c段    
  
> 旁站：是和目标网站在同一台服务器上的其它的网站  
> C段：和目标服务器ip位于同一C段的服务器  
  
嗅探旁站和C段的意义：旁站的意思就是在目标网站难以下手的情况下，从与目标网站在同一服务器的其他网站入手，提权，把服务器拿下，在该服务器下的目标网站自然也可以轻而易举地端掉了。至于C段，我们拿一个常见的IP:192.168.1.1作例子，看到IP分为四段，从左到右分为ABCD段，C段就是拿下它同一C段的其他服务器，也就是说D段1-255的其中一台服务器，然后再通过嗅探拿下该服务。  
   
旁站和c段的查询方式：  
```
（1）利用Bing.com，语法为：http://cn.bing.com/search?q=ip:192.168.1.1

（2）站长之家：http://s.tool.chinaz.com/same 

（3）利用Google，语法：site:192.168.1.*(找c段)

（4）利用Nmap，语法：nmap  -p  80,8080  –open  ip/24

（5）K8工具、御剑、北极熊扫描器等

（6）在线：http://www.webscan.cc/ 

（7）也可利用shodan搜索引擎搜索：net：192.168.1.1/24
```  
  
- - - 
参考资料：  

1.全流程信息收集方法总结  
2.白帽黑客的内网渗透教程
  

  

  

  

      

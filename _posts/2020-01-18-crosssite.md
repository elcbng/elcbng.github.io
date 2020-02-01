---
layout: post
title: xss漏洞详解1
tags: [crosssite]
lastUpdate: 2020-01-17
author:404player
---      
  

> 跨站脚本攻击xss(cross site scripting),为了不和层叠样式的缩写css混淆，故将跨站脚本攻击缩写为xss。xss漏洞的攻击原理是，恶意攻击者往往会在web页面中注入自己编写的恶意脚本代码(javascript,Java，VBscript甚至是普通的html代码)。当用户浏览这个页面的时候，恶意代码就会被触发，从而达到恶意攻击的目的。**xss本质上还是一种代码注入**，针对的是用户层面的攻击。   

出于介绍的目的，本文主要讨论了各种xss攻击的基本原理，也粗略提出几种预防xss攻击的方法。阅读本文要求读者由一定的前端开发语言(html,xss,javascript)知识，并不会对这些基础知识进行深入阐述，请想要了解的朋友可以先自行学习。    
  
<!--more-->  


## xss的原理与分类
- - -
xss主要分为三大类：存储型，反射性，dom型  
下面我们用图文的形式来深入剖析三种xss的异同：    

反射型**XSS**攻击原理：

![反射型xss攻击](https://img-blog.csdnimg.cn/20200117232521707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70)     

存储型**XSS**攻击原理:    

![存储型xss攻击](https://img-blog.csdnimg.cn/2020011800054667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70)    

Dom型**XSS**攻击原理： Dom型xss攻击本质上属于反射型xss攻击，所以示意图参考反射型xss攻击的示意图。区别在于要除去目标服务器的部分。  


**存储型**：存储型的xss是一种持久化的xss，为什么称之为持久化呢？是因为存储型的xss恶意代码是**存储在服务器中的**。在一些网页留言板，博客，评论区等地方，若网站没有对提交的文本进行过滤或编码，那么这些文本就会直接存储在目标服务器的数据库中，假若文本中带有黑客构造的恶意代码，普通用户在访问此页面的时候xss便会触发执行。存储型xss比较危险，常用于制造蠕虫，或者盗取cookie上。  


**反射型**: 反射型xss是一种非持久化的xss，需要黑客去精心伪造一个url(你也可以把它当作我们一般所说的网址)，并想方设法诱使用户去点击（常见的方法是我们在网页中随处可见的图片链接）。值得注意的是，反射型xss并不是存储在目标服务器中，它的恶意代码和所对应的页面都存储在黑客自己搭建的恶意服务器中。反射型xss一般出现在搜索页面上，一般用于盗取用户的cookie进行cookie欺诈。  

**Dom型**：要理解Dom型xss，首先要理解Dom的含义。Dom的全名为Docunment Objeet Model(文档对象模型)。你可以将之简略地理解成html中所有对象组成的树。Dom型xss就是基于文档对象模型的一种漏洞，因此，Dom型xss的触发是不需要经过后端服务器的。它是通过url传入参数直接触发，其实本质上也算是一种反射型的xss，攻击也是通过黑客构造的恶意链接进行。      
  
     


## 各种XSS攻击的实例演示    

为了让大家更好地理解各种XSS的攻击方式，下面我用本地搭建的环境来逐个演示一下。
- - -

### 反射型XSS实例演示   

![反射型](https://img-blog.csdnimg.cn/202001181001320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70)    

上面显示的页面是一个提交功能的form表单，我输入1提交以后，发现在提交框下部显示了"Hello+输入的内容"，而且仔细观察url后发现，url中的name参数正是输入的内容，由此判断页面存在反射型的XSS漏洞。    

为了确认页面真的存在反射型的XSS，我再次在输入框内写入``<script>alert(1)</script>``,点击submit后，出现下面所示的页面。    

![反射型测试](https://img-blog.csdnimg.cn/20200118100147371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70)   
发现页面出现了弹窗，确认此页面存在反射型的XSS！！！值得注意的一点是，我在输入框写入的脚本只是为了测试所用，实际情况下，弹出一个显示1的弹窗并没有什么实际作用，但是我们应该清楚的是，只要存在XSS漏洞，那么插入的恶意脚本就是黑客可以随意构造的，经过精心构造的脚本可以实行获取用户cookie或者被动提交其他敏感信息等严重威胁互联网安全的操作。  
- - -

### 存储型XSS实例演示    
<img src="https://img-blog.csdnimg.cn/20200118100157945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70" width = "1600" height = "600">    
  

上示页面就是我们生活中很常见的留言板网页，用户在登录情况下，只要在表单内(就是输入框)输入对应的文本且提交，用户输入就会保存在服务器数据库对应的个人数据下。我已经在admin用户下提交了几个文本(有些中文文本因为编码问题没有正常显示)。    

下面我们尝试在表单内输入构造的测试代码 < img src=# onerror="alert(1)" >并提交，再次访问留言板页面时发现生成了下面这样的页面。    

<img src="https://img-blog.csdnimg.cn/20200118100232511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70" width = "1600" height = "600">     
分析恶意代码，代码是一个图片属性，onerror表示当数据加载错误时触发，而当链接地址为#时，数据将一直为加载错误的状态，所以你会看到上图留言记录上有一张加载失败的图片与显示为1的弹窗。    

- - -

### Dom型XSS实例演示  
<img src="https://img-blog.csdnimg.cn/20200118100251412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70">    

上示页面也是一个普通的留言框，奇怪的是，当我输入hello以后，页面并没有发生任何的变化，就连url也没发现有什么异常。但作为一位安全测试人员，当然要学会从代码审计的角度去了解前端操作背后浏览器究竟进行了什么操作，于是我用Firefox的开发者工具看了一下页面的源代码，发现了一段有意思的代码:  


```html
<script>
    function test() {
        var str = document.getElementById("text").value;
        document.getElementById("t").innerHTML = "<h1><a href='" + str + "'>嘤嘤嘤你就是弹不出来</a></h1>";
    }
</script>
```  
  

这是一段javascript的代码，对编程语言有基本了解的同学很容易能猜测到，这段代码的作用其实就是定义一个变量，浏览器获取表单输入后赋值给这个变量，随后这个变量便作为``嘤嘤嘤你就是弹不出来``这段话的链接。通览整个实现过程，我们发现这段代码全部都由前端JavaScript输出，没有涉及到后端服务器的参数传递，这也是Dom型XSS跟反射型XSS最大的不同。    

清楚页面存在XSS后，我们要思考一下怎么构造payload(恶意代码)了，前面有说到，XSS本质上就是代码注入的一种，实现的原理就是将输入的数据当成代码来执行，所以要构造合适的payload，*我们就要想办法闭合代码中原本所带的标签，注释掉后面的代码，并在中间写入我们想要执行的恶意代码*。因为str变量前面有一个单引号，我们在构造时应先写入一个单引号，然后写入一个中括号去闭合前面的中括号。中间写入像前文一样的恶意代码，最后再加一个中括号去闭合str后面跟着的中括号。    


```html
'><img src=# onerror="alert(document.cookie)><"
```
所构造的恶意代码如上所示，提交后，生成页面：   

<img src="https://img-blog.csdnimg.cn/20200118100305135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dlZWtib3lfMTIz,size_16,color_FFFFFF,t_70">    

值得一提的是，这一段恶意代码的弹窗我不再想以前那样用显示1来测试，而是直接用弹出网页cookie来测试页面。cookie是某些网站为了验证用户身份，进行session跟踪而存储在用户本地终端上的数据，简单来说，我们平时登录网站后，进行页面跳转再次浏览该网站时不需要重新登录，就是cookie的功劳！获取了用户cookie，我们不但可以以用户的身份进行账号登录，还能够完成添加好友，发送信息等等只有用户本人才能进行的操作，这也就是前文所提到的cookie欺诈。    
     
- - -

## xss的注入点   
- - -   
- 把用户输入当成script标签的内容      

- 把用户输入当成html注释     

- 把用户输入当成html标签的属性名，属性值甚至直接当成标签的名字  

- 将用户输入插入CSS    

> **所有用户可控的数据都是不可靠的，开发者在进行开发的时候一定要对用户输入的东西进行过滤  
或者编码处理，且不要引入任何不可靠的第三方JavaScript。**     
  

  
  
  ```html
#所有的xss注入思维方式上往往都是闭合绕过，闭合前面的，注释或闭合后面的，中间写脚本  
  
#用户输入作为脚本  
<!--用户输入-->
<!--   (用户输入为 --><script>alert(document.cookie)</script><!-- )     -->  
#最后源代码输出的完整语句为：  
<!-- --><script>alert(document.cookie)</script><!-- -->  
   
#用户输入当成html属性名  
<form 用户输入="~~~"></form>  
#经闭合绕过后完整语句为：  
<form></form><script>alert(1)</script><form input="a"></form>  
  
#用户输入当成html属性值
<div id="用户输入"></div>  
#经闭合绕过以后  
<div id=""></div><script>alert("PWN")</script><div a="aha"></div>  
  
#用户输入作为标签名  
<用户输入 a="ahaha">  
#经闭合绕过以后  
<><script>alert(1)</script><div a="ahaha"></div>  
  
#用户输入作为CSS内容输出  
<style>用户输入</style>  
<style></style><script>alert(1)</script><style></style>  
  
## 此处所用所有html标签都是笔者自选的，但只要通晓原理，XSS其实是一种可以很灵活运用的漏洞
## 此处xss注入点的编写借鉴了九歌的文章  
```

- - -


## XSS漏洞的挖掘  
- - -     

对漏洞的挖掘往往要有针对性，我们要清楚某种漏洞经常在什么地方可被利用，才能更好地挖掘出对应的漏洞，同样，要了解XSS漏洞的挖掘，我们必须要清楚XSS漏洞的重灾区。前面提到，XSS漏洞本质上还是一种代码注入，而且发生在前端用户输入中，可以联想到，留言区、评论区、搜索框等都是XSS漏洞的重灾区。  
    
      
相信考过计算机等级考试二级的同学都知道，软件测试分为两种测试：黑盒测试和白盒测试。黑盒测试，顾名思义，就是在不清楚内部逻辑(不给出源代码)的情况下，着眼于外部结构去测试程序是否满足规格说明书的要求。反之，白盒测试则是代码审计，检查编写代码中是否出现逻辑或者技术层面的错误。漏洞的挖掘也是如此，分为黑盒与白盒的测试，灵活运用两种测试方法，才能有效地挖掘出漏洞。  

### 黑盒测试    

由于黑盒测试实在外部程序接口地方进行测试，前文也提到了，XSS漏洞的触发会在页面输入框和url中产生影响，所以输入与url便是我们在黑盒测试的时候应该关注的地方。  

- 要关注url提交给服务器的参数(也就是？后面的参数)   
 
- url的锚点(#后面的东西)及其他部分的变化也是值得注意的    

- 关注表单提交的时候页面的变化   
  
### 白盒测试

由于涉及代码逻辑层的东西，白盒测试比起黑盒测试就有了更多可说的地方了。关于XSS的代码审计要从参数的传递入手，要观察参数经浏览器输入后传入与输出的地方以及中间过程中是否有经过过滤和过滤的方式。  

网站中参数传递一般由PHP代码完成，PHP接收参数的方式一般由$_GET,$_POST,$_REQUEST等等，测试者在审计源代码的时候可以善用搜索工具直接定位到传参的地方，去对传入的参数进行跟踪，特别要注意看看参数会不会输出到页面中，以及输出的数据是否进行了过滤和编码等处理。  

当然也可以换一种思路，刚刚我们采用的是从源头追踪去处的方法。有时候不妨可以采取一下从终点溯源的方法。像PHP的echo函数，就是起在页面中输出数据的作用，我们定位到echo的位置后，可以追踪echo的数据在输出前是否经过过滤或者编码，又或者我们直接追踪echo数据的来源，看看它在输出前来源是否可靠等等。    

当然，不要忘了还有一种只有javascript触发的Dom型XSS，我们定位这种类型XSS的时候，可以搜索一下js操作Dom元素的函数或者方法。  

- - -

## xss的危害   

- 盗用用户的cookie，进行cookie欺诈获得用户的敏感信息    

- 盗用管理员身份后，可进行提权或者其他危害更大的行为    

- 利用受攻击域收到其他域信任的特点，以受信任的身份请求一些平时不允许的操作，如进行不正当的投票。    

- 在访问量较大的网站中如果存在XSS，可以植入恶意脚本请求一些小网站，对小网站执行DDos攻击  
  
## xss的防御

前文提到了，XSS漏洞是由于没有对用户输入进行过滤造成的，所以防御的思路很简单———使网站不相信任何用户可控的数据，即对输入进行过滤，对输出进行编码。  


1.对输入进行过滤    

>具体的方法就是针对具有特定格式的输入(如电话号码，身份证号或者是账号密码等只有数字与字母的)，要对输入进行白名单过滤，也就是说输入如果不是数字或者字母，则提示输入错误，要重新输入。针对留言板或者评论区等各种字符集混杂的输入框呢？就要学会用黑名单加以限制，比如限制输入特殊的html标签与javascript事件属性等等，或者直接过滤掉<>.  
  

2.html 实体    

当需要在html标签中插入不可信任的数据时，需要先对不可信任数据进行HTML Entity编码。简单来说，就是对有可能引发XSS的一些符号用一种编码以代替，再插入html标签中。  
  


|显示结果|解释|实体编号|
|-------|----|-------|
|       |空格|&nbsp ;|
|<      |小于|&lt ;  |
|>       |大于|&gt ;|
|&       |和  |	&amp ;|
|"       |引号|	&quot ;|


3.javascript编码    

针对动态生成的javascript代码，我们也需要在将数据插入其中的时候对其进行编码(上述dom型xss就是没有进行编码的结果)。javascript编码就是针对插入JavaScript代码中数据安全性的一种编码。  
  

> \ 转成 \\\    
 / 转成\ \/ 转成  
  ；(全角;)    

 > 注意：在对不可信数据做编码的时候，不能图方便使用反斜杠\ 对特殊字符进行简单转义，比如将双引号 ”转义成 \”，这样做是不可靠的，因为浏览器在对页面做解析的时候，会先进行HTML解析，然后才是JavaScript解析，所以双引号很可能会被当做HTML字符进行HTML解析，这时双引号就可以突破代码的值部分，使得攻击者可以继续进行XSS攻击；另外，输出的变量的时候，变量值必须在引号内部，避免安全问题；更加严格的方式，对除了数字和字母以外的所有字符，使用十六进制\xhh 的方式进行编码。————选自前端潘潘的*WEB 安全之 XSS 攻击与防御*  


4.使用http only cookie    

cookie是验证用户身份的重要信息，黑客进行XSS攻击时目的多是盗取用户cookie进行cookie欺诈。所以重要的cookie可将其标志为http only。标记后，并不会影响浏览器在向服务器发起请求的时候带上cookie验证，但是javascript中的docunment.cookie却再也没有办法获取到用户的cookie了。(我们平时可以在开发者工具的console中输入```document.cookie```获取当前页面的cookie)

所谓道高一尺魔高一丈，其实并没有绝对安全的系统，上述防御方法当然可以防御一部分的XSS漏洞，但是有了新的防御方法，同样恶意攻击者们也会继续研究出新的绕过方法。想要一劳永逸的解决安全问题终究还是异想天开，所以作为网络安全的研究者，我们必须不断地学习，不断地吸取新知识，才能够在新科技的浪潮中找到自己的一席之地，加油！！！  


## 参考资料 
- 公众号HACK  
- csdn论坛  
-   WEB 安全之 XSS 攻击与防御  

##以下为极客时间的两日专题课程-《网络协议集训班》的内容总结 共15课时

  

# 课时一 在浏览器上输入URL到主页显示，经历了哪些步骤  
 

# 课时二 如何抓包分析网络报文   
**普通文本(plain text)也是正则表达式，匹配其本身**  

**grep略影**  
grep是一种用来对文件或标准输入文本进行文字搜索的Unix工具。  
grep支持基本(-G)、扩展(-E)和Perl(-P)正则表达式。  
**e.g.**   
我们想在一句话（Hello，my name is aming.)中匹配中间的一段字符串（my name is) 可以这样写正则表达式:  
```shell
echo "Hello, my name is aming."|grep -P '(?<=Hello, ).*(?= aming.)'
```   
- 默认情况下，grep将把包含匹配的各个文本行全部显示出来，如果只想看匹配结果，使用-o选项  
- 使用-v选项将对操作取反，也就是只显示不匹配的文本行  
- 使用-c选项将只显示匹配的数量而不是匹配到的具体内容  
- 使用-i选项进行不区分字母大小写的匹配  
- grep工具只用来搜索，无法进行替换  

**元字符 .**  
用来匹配任意单个字符  
 
**元字符\\**  
转义字符，总是出现在具有特殊含义字符序列的开头，这个序列可以由一个或者多个字符构成    
  

# 课时三 RESTful API该如何设计  
**元字符[和元字符]**   
- 使用元字符[和元字符]来定义一个字符集合。出现在其中的所有字符都是该集合的组成部分，必须匹配其中某个成员。  

`示例文本`   
```shell
sales1.xls  
orders3.xls  
sales2.xls  
sales3.xls 
apac1.xls   
europe2.xls  
na1.xls  
na2.xls  
sa1.xls  
ca1.xls  
```
`正则表达式`  
```shell 
[ns]a.\.xls  
```
`结果(只显示匹配项)`  
```shell 
na1.xls  
na2.xls  
sa1.xls  
``` 
**特殊元字符 -**  

- 使用正则表达式会频繁使用一些字符区间(0-9、A-Z、a-z等)，为了简化区间定义，提供元字符-来定义字符区间   
- 字符区间的首、尾字符可以是ASCII字符表中的任意字符，但不能首大于尾(无意义)    
- -只有出现在元字符[]之间的时候才是元字符，其他地方只是普通字符，无需转义   
- 同一个集合中可以出现多个字符区间，e.g. [A-Za-z0-9]  
      
`示例文本`   
```shell
sales1.xls  
orders3.xls  
sales2.xls  
sales3.xls 
apac1.xls   
europe2.xls  
na1.xls  
na2.xls  
sa1.xls  
ca1.xls  
```
`正则表达式`  
```shell 
[ns]a[0-9]\.xls  
```
`结果(只显示匹配项)`  
```shell 
na1.xls  
na2.xls  
sa1.xls  
```   

**元字符 ^**  
- 某些场合需要制定一组不需要匹配的字符，即排除。使用元字符^来排除某个字符集合  
- ^的效果将作用于给定字符集合里的所有字符或者字符区间，故^在字符集合之首   
 
`示例文本`   
```shell
sales1.xls  
orders3.xls  
sales2.xls  
sales3.xls 
apac1.xls   
europe2.xls  
sam.xls  
na1.xls  
na2.xls  
sa1.xls  
ca1.xls  
```
`正则表达式`  
```shell 
[ns]a[^0-9]\.xls  
```
`结果(只显示匹配项)`  
```shell 
sam.xls    
```  
    

# 课时四 哪些HTTP头部配合浏览器增强了Web安全

**空白元字符：**  

| 元字符 | 说明 |
| ----- | ----- |
| \b | 回退字符 |
| \f | 换页符 |
| \n | 换行符 |
| \r | 回车符 |
| \t | 制表符Tab |
| \v | 垂直制表符 | 
| \s | 任何一个空白字符，等价于[\f\n\t\r\v]，不包含\b |
| \S | 任何一个非空白字符，等价于[^\f\n\t\r\v]，不排除\b | 

**字母数字元字符：**  

| 元字符 | 说明 |
| ----- | ----- |
| \d | 任何一个数字字符，等价于[0-9] |
| \D | 任何一个非数字字符，等价于[^0-9] |
| \w | 任何一个字母(大小写皆可)数字字符或下划线字符，等价于[a-z0-9A-Z_] |
| \W | 任何一个非字母数字字符或下划线字符，等价于[^a-z0-9A-Z_] |
| \x | 匹配一个十六进制数值，e.g. \x0A |
| \ | 匹配一个八进制数值，e.g. \141 |   

**POSIX字符集：**  

| 元字类 | 说明 |
| ----- | ----- |
| [:alnum:] | 任何一个字母(大小写皆可)数字字符，等价于[a-z0-9A-Z] |
| [:alpha:] | 任何一个字母(大小写皆可)，等价于[a-zA-Z] |
| [:blank:] | 空格或制表符，等价于[\t ] |
| [:cntrl:] | ASCII控制字符，0~31，再加上127 |
| [:digit:] | 任何一个数字，等价于[0-9] |
| [:graph:] | 和[:print:]一样，但不包括空格 |
| [:lower:] | 任何一个小写字母，等价于[a-z] |
| [:print:] | 任何一个可打印字符 |
| [:punct:] | 既不属于[:alnum:]，也不属于[:cntrl:]的任何字符 |
| [:space:] | 任何一个空白字符，包含空格，等价于[\f\n\t\r\v ] |
| [:upper:] | 任何一个大写字母，等价于[A-Z] |
| [:xdigit:] | 任何一个十六进制数字，等价于[a-f0-9A-F] | 
  
`示例文本`   
```shell
#fefbd8
#0000ff
#d0f4e6
#f08970  
```
`正则表达式`  
```shell 
#[[:xdigit:]][[:xdigit:]][[:xdigit:]][[:xdigit:]][[:xdigit:]][[:xdigit:]]  
```
`结果(只显示匹配项)`  
```shell 
#fefbd8
#0000ff
#d0f4e6
#f08970    
```  

**注意：**    
使用[[]]两对方括号，是使用POSIX字符类所必须的，这点很重要。POSIX字符类必须出现在[:和:]之间，外层的[]为字符集合元字符。   


# 课时五 WAF防火墙究竟是怎样工作的？  

**5.1 匹配一个或多个字符**  

使用元字符+  


- +匹配某个字符或字符集合的一次或多次重复   

`示例文本`   
```shell
send email to ben@forta.com or 
ben.forta@forta.com. For questions about a 
book use support@forta.com. If your message  
is urgent try ben@urgent.forta.com. Feel free  
to send unsolicited email to 
spam@forta.com(wouldn't it be nice if 
it were that simple, huh?). 
```
`正则表达式`  
```shell 
[\w.]+@[\w.]+\.\w+  
```
`结果(只显示匹配项)`  
```shell 
ben@forta.com 
ben.forta@forta.com
support@forta.com
ben@urgent.forta.com
spam@forta.com   
```   

**5.2 匹配零个或多个字符**  

使用元字符*  


- *匹配某个字符或字符集合的零次或多次重复   

`示例文本`   
```shell
hello .ben@forta.com is my email address. 
```
`正则表达式`  
```shell 
[\w]+[\w.]*@[\w.]+\.\w+  
```
`结果(只显示匹配项)`  
```shell 
ben@forta.com  
```   

**5.3 匹配零个或一个字符**  

使用元字符?  


- ?匹配某个字符或字符集合的零次或一次出现，最多不超过一次，非常适合匹配一段文本中某个特定的可选字符     

`示例文本`   
```shell
The URL is http://regex101.com/, to connect 
securely use https://regex101.com/ instead.
```
`正则表达式`  
```shell 
https?:\/\/[\w.\/]+ 
```
`结果(只显示匹配项)`  
```shell 
http://regex101.com/ 
https://regex101.com/ 
```    

**5.4 匹配的重复次数**  

**鉴于5.1-5.3使用的元字符存在以下局限：**   


- +和*匹配的字符个数没有上限,无法设定匹配字符个数的最大值
- +、*、?匹配的字符最小数目是0或者1个,无法设定匹配字符个数的最小值
- 无法指定具体的匹配次数  

**正则表达式允许使用重复范围(interval)，使用元字符{和}指定**     


- 可指定具体的匹配次数，e.g. #[[:xdigit:]]{6} 参见P46实例
- 可指定期望的区间范围，e.g. \d{1,2}[-\/]\d{1,2}[-\/]\d{2,4} 参见P47实例
- 可指定至少重复多少次，e.g. \d+: \$\d{3,}\.\d{2} 参见P48实例

**5.5 防止过度匹配**  

- *和+是所谓“贪婪型(greedy)元字符(也称为量词quantifier)”，其匹配行为会尽可能从一段文本的开头一直匹配到末尾，而非匹配到第一个就停止
- 在不需要这种贪婪行为时，改用量词的懒惰型版本，匹配尽可能少的字符，使用方式是在量词后加?


| 贪婪型量词 | 懒惰型量词 |
| ----- | ----- |
| * | *? |
| + | +? |
| {n,} | {n,}? |  


`示例文本`   
```shell
This offer is not available to customers 
living in <b>AK</b> and <b>HI</b>.
```
`正则表达式`  
```shell 
<[Bb]>.*?<\/[Bb]> 
```
`结果(只显示匹配项)`  
```shell 
<b>AK</b> 
<b>HI</b>
```    
 

# 课时六 HTTP/1协议该如何优化性能？ 

**6.1 单词边界**  

**正则表达式提供了专用的单词边界（world boundary），记为\b。它匹配的是“单词边界”位置，而不是字符。也就是说，\b能够匹配这样的位置：一边是单词字符，另一边不是单词字符。**  
清晰的附图描述，见：https://www.cnblogs.com/gaara0305/p/10027343.html  



- 一般情况下，“单词字符”的解释是\w能匹配的字符。也即\b的位置受限于\w的定义。在JavaScript、PHP、Python 2、Ruby中，\w只能匹配[0-9A-Za-z_]，所以像e-mail、M.I.T、抑或是中文单词等无法符合匹配条件，所以应该结合具体的使用环境设计正则表达式，避免匹配结果不符合预期  
- 与单词边界\b对应的是非单词边界\B，两者的关系是，同一种语言中，不管\b是如何规定的，\b能匹配到的位置，\B就不能匹配，反之亦然  

`示例文本`   
```shell
Please enter the nine-digit id as it 
appears on your color - coded pass-key.
```
`正则表达式`  
```shell 
\B-\B
```
`结果`  
```shell 
"color - coded"处的-会被匹配到，"nine-digit"中的-并不被匹配
```   

**6.2 字符串边界**    

**正则表达式提供了字符串边界，用于在字符串的首位进行模式匹配。字符串边界元字符有两个:^代表字符串开头，$代表字符串结尾。**  
**字符串的定义，当前默认为整个完整文本** 

`示例文本`   
```shell
<?xml version="1.0" encoding="UTF-8" ?>
<wsdl:definitions targetNamespace="http://tips.cf" 
xmlns:impl="http://tips.cf" xmlns:intf="http://tips.cf"
xmlns:apachesoap="http://xml.apache.org/xml-soap"
```
`正则表达式`  
```shell 
^\s*<\?xml.*\?>
```
`结果`  
```shell 
<?xml version="1.0" encoding="UTF-8" ?>
```  

**6.3 字符串边界多行模式**   

**多行模式迫使正则表达式引擎将换行符视为字符串分割符，这样一来。^既可以匹配字符串的开头，也可以匹配换行符之后的起始位置；$不仅可以匹配字符串结尾，还能匹配换行符后的结束位置**   

**使用(?m)启用多行模式，(?m)必须出现在整个模式的最前面**  

`示例文本`   
```C
int lengthOfLastWord(char * s){
	int i,cnt=0;
	
	// judge 1
	if (!s || strlen(s) == 0)
		return 0;

	i = strlen(s)-1;

	while (i >= 0 && s[i] == ' ')
		i--;

	// judge 2
	if (i == 0 && s[i] == ' ')
		return 0;

	while (i >= 0 && s[i] != ' ')
	{
		cnt++;
		i--;
	}

	return cnt;
}

```
`正则表达式`  
```shell 
(?m)^\s*\/\/.*$
```
`结果`  
```C 
// judge 1
// judge 2
```  

# 课时七 升级到HTTP2协议有哪些好处？

**7.1 字符串边界多行模式**     

**之前章节介绍的表明重复次数的元字符(?、*、{2}等)只作用于紧挨着它的前一个字符或元字符，为了是更长的表达式能被作用到，使用子表达式(和)**  

`示例文本`   
```shell
Hello, my name is Ben&nbsp; Forta, and I am 
the author of multiple books on SQL (including 
MySQL, Oracle PL/SQL, and SQL Server T-SQL), 
Regular&nbsp;&nbsp; Expressions, and other subjects.

```
`正则表达式`  
```shell 
(&nbsp;){2,}
```
`结果`  
```C 
&nbsp;&nbsp;
```   

**OR语义操作符：|**   

|是OR(或)操作符，e.g. 19|20可以匹配19或者20  
(19|20)\d{2} 可以匹配19XX或者20XX的年份(**注意|涉及优先权问题，19|20\d{2}就不能完成前述的功能**)  

**注意多个可匹配时的语句排列，因为模式是从左到右进行匹配的，所以如果多个表达式都可以匹配，则第一个匹配到即完成匹配**  

`示例文本`   
```shell
Pinging hog.forta.com [12.159.46.200]
with 32 bytes of data:
```
`正则表达式`  
```shell 
(((25[0-5])|(2[0-4]\d)|(1\d{2})|(\d{1,2}))\.){3}((25[0-5])|(2[0-4]\d)|(1\d{2})|(\d{1,2}))
```
`结果`  
```C 
12.159.46.200
```   

上例中，如果将内部的四个OR顺序替换，则可能出现不符合预期的匹配结果：  

`示例文本`   
```shell
Pinging hog.forta.com [12.159.46.200]
with 32 bytes of data:
```
`正则表达式`  
```shell 
(((\d{1,2})|(1\d{2})|(2[0-4]\d)|(25[0-5]))\.){3}((\d{1,2})|(1\d{2})|(2[0-4]\d)|(25[0-5]))
```
`结果`  
```C 
12.159.46.20  [最后一位的200并没有完整匹配到]
```   

**问题： AND语义如何实现呢？**   
**比如上例中的\d{1,2}并没有排除00这种无效数字字符串，考虑使用AND语义排除之？ 当然不实现AND也可以做啦**  


# 课时八 下一代的HTTP3协议有哪些新特性？

# 课时九 TLS/SSL协议使用了哪些密码学知识？ 

# 课时九 TLS/SSL协议使用了哪些密码学知识？

# 课时十 如何优化TLS/SSL协议的性能、安全性？

# 课时十一 为何TCP连接要经由三次握手才能建立？

# 课时十二 如何优化TCP协议的性能？ 

# 课时十三 TCP的自动分包会引入哪些性能问题？

# 课时十四 为什么必须使用UDP协议发送广播报文？

# 课时十五 如何抓包分析K8S容器网络中的报文？

# 课时十六 
 
**8.1 反向引用**

理解为什么需要反向引用，可举例说明。     
HTML使用标题标签(h1~h6,以及配套的结束标签)来排版Web页面的标题文字，假设要把所有标题文字查找出来，不管几级标题，我们首先尝试下面的方式：  

`示例文本`   
```shell
<body>
<h1>Welcome to my Homepage</h1>
Content is divided into two sections:<br/>
<h2>SQL</h2>
Information about SQL.
<h2>RegEx</h2>
Information about Regular Expressions.
</body>
```
`正则表达式`  
```shell 
<[Hh][1-6]>.*?<\/[Hh][1-6]>
```
`结果`  
```shell 
<h1>Welcome to my Homepage</h1>
<h2>SQL</h2>
<h2>RegEx</h2>
```  

上面的例子似乎功能OK，但如果某一行的标题格式书写有误如下：  

```shell 
<h2>RegEx</h3>
```
上述的正则表达式仍会匹配该非法行。  

**问题在于匹配的第二部分(用来匹配结束标签的那一部分)对匹配的第一部分(用来匹配开始标签的那部分)一无所知。这正是反向引用大显身手的地方。**    

反向引用的场景：
**我们使用反向引用，来匹配和某一子表达式相同的内容**   

反向引用的用法：
**\1匹配模式中使用的第一个子表达式，\2匹配第二个子表达式，\3匹配第三个，以此类推**   

使用反向引用，不符合语法的非法行不会被匹配到：  
  
`示例文本`   
```shell
<body>
<h1>Welcome to my Homepage</h1>
Content is divided into two sections:<br/>
<h2>SQL</h2>
Information about SQL.
<h2>RegEx</h3>
Information about Regular Expressions.
</body>
```
`正则表达式`  
```shell 
<([Hh][1-6])>.*?<\/\1>
```
`结果`  
```shell 
<h1>Welcome to my Homepage</h1>
<h2>SQL</h2>
```  

**注意1**  
正则表达式实现中，反向引用的语法各有差异，请确认后使用。e.g. Perl使用过$1而非\1来代表反向引用。  

**注意2**  
有些正则表达式支持“命名捕获(named capture)”，即给某个子表达式起一个唯一的名字，随后可使用该名称(而非相对位置)来引用此子表达式。

**8.2 替换操作**  

本书前述内容都在讲搜索，正则表达式还可以用来完成各种替换操作。 

看一个例子。在一个保存用户信息的数据库中，电话号码的保存格式为313-555-1234。现在需要调整格式为(313) 555-1234。

`示例文本`   
```shell
313-555-1234
248-555-9999
810-555-9000
```
`正则表达式`  
```shell 
(\d{3})(-)(\d{3})(-)(\d{4})
```
`替换`
```shell 
(\1) \3-\5
```

`结果`  
```shell 
(313) 555-1234
(248) 555-9999
(810) 555-9000
```  

**除了直接取出替换，还可以进行字母的大小写转换**   


| 元字符 | 说明 |
| ----- | ----- |
| \E | 结束\L或\U |
| \l | 放置在字符或子表达式之前，把下一个字符转化为小写 |
| \L | 转化其与\E之前的所有字符为小写 | 
| \u | 放置在字符或子表达式之前，把下一个字符转化为大写 |
| \U | 转化其与\E之前的所有字符为大写 |  

`示例文本`   
```shell
<body>
<h1>Welcome to my Homepage</h1>
Content is divided into two sections:<br/>
<h2>SQL</h2>
Information about SQL.
<h2>RegEx</h3>
Information about Regular Expressions.
</body>
```
`正则表达式`  
```shell 
(<[Hh]1>)(.*?)(<\/[Hh]1>)
```
`替换`
```shell 
\1\U\2\E\3
```
`结果`  
```shell 
<h1>WELCOME TO MY HOMEPAGE</h1>
```  

# 第九章 环视 




# 第十章 嵌入式条件  
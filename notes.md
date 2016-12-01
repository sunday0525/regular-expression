#正则表达式--(Regular Expression)
>首先：正则表达式只是用于字符串的(擅长匹配模糊范围的字符串)；是一种规则
正则调试工具： http://www1.w3cfuns.com/tools.php?mod=regex

##正则写法
直接量语法：
`var re = /pattern/attributes`
例：
`var re = /ad/g`

创建 RegExp 对象的语法：
`var re = new RegExp(pattern, attributes);`
例：
`var re = new RegExp("ad","g")`


1.参数:
    参数 pattern 是一个字符串，指定了正则表达式的模式或其他正则表达式。
    参数 attributes 是一个可选的字符串(修饰符)，包含属性 "g"、"i"和"m"，分别用于指定全局匹配、区分大小写的匹配和多行匹配。ECMAScript 标准化之前，不支持m属性。
    如果pattern是正则表达式，而不是字符串，则必须省略该参数。如：`var re = new RegExp(/\d/);`
2.返回值:
    一个新的RegExp对象，具有指定的模式和标志。如果参数pattern是正则表达式而不是字符串，那么RegExp()构造函数将用与指定的RegExp相同的模式和标志创建一个新的 RegExp 对象。
    如果不用 new 运算符，而将 RegExp() 作为函数调用，那么它的行为与用 new 运算符调用时一样，只是当 pattern 是正则表达式时，它只返回 pattern，而不再创建一个新的 RegExp 对象。
3.抛出：
    SyntaxError - 如果 pattern 不是合法的正则表达式，或 attributes 含有 "g"、"i" 和 "m" 之外的字符，抛出该异常。
    TypeError - 如果 pattern 是 RegExp 对象，但没有省略 attributes 参数，抛出该异常。


>注：
1.简写的性能更好，但是在有些特殊的情况下只能使用全称；
2.正则中接受的是字符串，只不过在简写中不加""。如：`ad`;
3."//"中不能为空不然会被识别成注释。

##转义字符"\"

目的：将一些不能直接显示的字符通过转义字符显示出来,将一些asc码转译成为特殊功能的符号;
比如："n"前添加"\"的话,"n"就不是"n"了,而是成为了换行符。而若是想输出"\n",不能直接写"\n",否则会被识别成换行,只有写成"\\\n"才能真正的显示"\n";

| 或者   同js中  ||

##元字符

元字符（Metacharacter）是拥有特殊含义的字符(所以可以作为字符串写在RegExp中)：
例如：`var re = new RegExp("\w")` 参数必须是字符串，遇\必须要转义，即写两个\（字符串里有\也会转义，需慎重）  直接量语法：`var re = /\w/`
**.** 任意字符，除了换行和行结束符。  例： var str = 'a bc d'   str.match(/./g) => a,"",b,c,"",d
**\w** 查找单词字符(数字、字母、下划线)。**\W** 查找非单词字符。 (word)
**\d** 查找数字。**\D** 查找非数字字符。(digit)
**\s** 查找空白字符。**\S** 查找非空白字符。(space)   var str = str.replace(/^\s+|\s+$/g,'');  //去掉开头结尾空格
**\b** 匹配单词边界(独立的部分，起始；空格；结束) 字母、数字、_有 ，不认中文。
>例：
var str = "my name is li"
var re = new RegExp('\\b'+ 变量 + '\\b','g') == new RegExp('^|\\s'+ 变量 + '\\s|$','g') 

var r = str.replace(/^\s+|\s+$/g,'') 去除首尾空格

**\B** 匹配非单词边界。(bound)
**\0** 查找 NUL 字符。
**\n** 查找换行符。
**\f** 查找换页符。
**\r** 查找回车符。(return)
**\t** 查找制表符。(table)
**\v** 查找垂直制表符。(vertical)
**\xxx** 查找以八进制数 xxx 规定的字符。
**\xdd** 查找以十六进制数 dd 规定的字符。
**\uxxxx** 查找以十六进制数 xxxx 规定的 Unicode 字符。

中文范围：[ \u4e00 - \u9fa5 ]
var str = '你好，世界，hello' //字符串的占位长度
for(var i=0; i<str.length;i++){
	if(/[\u4e00-\u9fa5]/.test(str.charAt(i))){
		num+=3;
	}else{
		num++;
	}
}



##修饰符

i 执行对大小写不敏感的匹配。
g 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
m 执行多行匹配。

##量词
量词指的都是量词前一个字符而不是字符串的数量，如果需要使用一串字符，就需要使用"()"化为子项。
量词是用于匹配不确定的位置的字符。

- n+ 匹配任何包含至少一个 "n" 的字符串，相当于{1,}。
例如：/\w+/匹配的就是任意位数的字符；
- n* 匹配任何包含至少0个 "n" 的字符串。
相当于n{0,}
实例：
```
    var str="Hellooo World! Hello W3School!";
    var patt1=/lo*/g;
	str.match(patt1) =>l,looo,l,lo,l
	str.match(/lo/g) => lo,lo
```
解释： 首先找到所有的"l",然后循环找到带有"o"和不带有"o"的字符串。而带有除"o"以外字符的将会被抛弃。

- n{X} 匹配包含 X 个 "n" 的序列的字符串。
- n{X,Y} 匹配包含 X -- Y 个 "n" 的序列的字符串。
- n{X,} 匹配包含至少 X 个 "n" 的序列的字符串。
- n{0,} *
- n{0,1} ?
- n{1,} +
- n$ 匹配任何结尾为 "n" 的字符串。
- ^n 匹配任何开头为 "n" 的字符串。
- string(?=n) 匹配任何其后紧接字符串 "n" 的string。
- string(?!n) 匹配任何其后没有紧接字符串 "n" 的string。
```
语法
new RegExp("regexp(?=n)")
直接量语法：
/regexp(?=n)/
```

##方括号--正则表达式中的字符类
方括号用于查找某个范围内的字符：

表达式 描述
- [abc] 查找方括号之间的任一字符（一个）。
- [^abc] 查找任何不在方括号之间的字符。
- [0-9] 查找任何从 0 至 9 的数字。
- [a-z] 查找任何从小写 a 到小写 z 的字符。
- [A-Z] 查找任何从大写 A 到大写 Z 的字符。
- [A-z] 查找任何从大写 A 到小写 z 的字符。
- [adgk] 查找给定集合内的任何字符。
- [^adgk] 查找给定集合外的任何字符。
- (red|blue|green) 查找任何指定的选项。

去标签实例：
`var re = /<[^]+>/g`
<a href="正则11.html" target="_blank">实例</a>

>注：
1.一个[]只是表达一个字符，里面的字符是或的关系。
2.()有两个意思。
- 分组操作；
    相当于运算符中的()
- 匹配操作
    主要使用于repalce和match
    

##正则的方法

- test  => 布尔值
- search  => 位置（下标）/-1
- match   => 数组（把匹配出的字符存入一个数组）/ null
- replace  => 替换后的字符串（不影响原字符串）

###1.test
**test**规则：
拿正则去匹配字符串，若是匹配成功则返回true，否则false；

**test**写法
`RegExpObject.test(string)`
RegExpObject--是指正则对象。
string--是需要检测匹配的字符串。

```
    var re = new RegExp(/\d/);
    //var re = /\d/;
    oBtn.onclick = function(){
        var str = oText.value;
        oSpan.innerHTML = re.test(str);
    }

验证QQ：5-11位纯数字，不能以0开头
var re = /^[1-9]\d{4,10}$/.test(str)

```
<a href="正则1.html" target="_blank">实例</a>


###2.search

**search规则**
拿正则去匹配字符串,若成立则返回stringObject中第一个与 regexp 相匹配的子串的起始位置,否则则返回-1；

**search写法**
`stringObject.search(regexp)`

其中：**regexp**该参数可以是需要在 stringObject 检索的子串，也可以是需要检索的 RegExp 对象。

```
    var re = /\d/;
    oBtn.onclick = function(){
        var str = oText.value;
        oSpan.innerHTML = oText.value.search(re);
    }
```

<a href="正则3.html" target="_blank">实例</a>

>注：
要执行忽略大小写的检索，请追加标志 i。
search() 方法不执行全局匹配，它将忽略标志 g。它同时忽略 regexp 的 lastIndex 属性，并且总是从字符串的开始进行检索，这意味着它总是返回 stringObject 的第一个匹配的位置。


###3.math

**math规则**
拿正则去匹配字符串，若成功则返回存放匹配结果的数组。该数组的内容依赖于 regexp 是否具有全局标志 g，失败则返回null；

**math规则**

```
stringObject.match(searchvalue)
stringObject.match(regexp)
```

>参数：
searchvalue 必需。规定要检索的字符串值。
regexp 必需。规定要匹配的模式的 RegExp 对象。如果该参数不是 RegExp 对象，则需要首先把它传递给 RegExp 构造函数，将其转换为 RegExp 对象。

```
    var re = /\d/g;
    oBtn.onclick = function(){
        var str = oText.value;
        oSpan.innerHTML = oText.value.match(re);
        // oSpan.innerHTML = oText.value.match(/\d/g);
        // oSpan.innerHTML = oText.value.match("a");
    }
```
<a href="正则4html.html" target="_blank">实例</a>

**match与匹配子项--()**

在没有标识符"g"的情况下，match与()配合会返回匹配成功的子项集合。
例
```
        var re = /(a)(b)(c)/
        oBtn.onclick = function(){
            oText[1].value = "abc".match(re); //[abc,a,b,c]
        }
```

<a href="正则13.html">实例</a>


>注：
1.match() 方法将检索字符串 stringObject，以找到一个或多个与 regexp 匹配的文本。这个方法的行为在很大程度上有赖于 regexp 是否具有标志 g。
2.如果 regexp 没有标志 g，那么 match() 方法就只能在 stringObject 中执行一次匹配。如果没有找到任何匹配的文本， match() 将返回 null。否则，它将返回一个数组，其中存放了与它找到的匹配文本有关的信息。该数组的第 0 个元素存放的是匹配文本，而其余的元素存放的是与正则表达式的子表达式匹配的文本。除了这些常规的数组元素之外，返回的数组还含有两个对象属性。index 属性声明的是匹配文本的起始字符在 stringObject 中的位置，input 属性声明的是对 stringObject 的引用。
<a href="正则5html.html" target="_blank">实例</a>
3.如果 regexp 具有标志 g，则 match() 方法将执行全局检索，找到 stringObject 中的所有匹配子字符串。若没有找到任何匹配的子串，则返回 null。如果找到了一个或多个匹配子串，则返回一个数组。不过全局匹配返回的数组的内容与前者大不相同，它的数组元素中存放的是 stringObject 中所有的匹配子串，而且也没有 index 属性或 input 属性。

注意：在全局检索模式下，match() 即不提供与子表达式匹配的文本的信息，也不声明每个匹配子串的位置。如果您需要这些全局检索的信息，可以使用 RegExp.exec()。

###4.replace

**replace规则**
方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。返回替换后的新字符串不会改变原来的字符串。

**replace写法**
`stringObject.replace(regexp/substr,replacement)` 如果没有第二个参数，或第二个参数是函数但没有返回值时，replace返回值都是undefined

**replace说明**

1.字符串 stringObject 的 replace() 方法执行的是查找并替换的操作。它将在 stringObject 中查找与 regexp 相匹配的子字符串，然后用 replacement 来替换这些子串。如果 regexp 具有全局标志 g，那么 replace() 方法将替换所有匹配的子串。否则，它只替换第一个匹配子串。

2.replacement 可以是字符串，也可以是函数。
  1）如果它是字符串，那么每个匹配都将由字符串替换。但是 replacement 中的 $ 字符具有特定的含义。它说明从模式匹配得到的字符串将用于替换。 如果是函数，必须有一个返回值
  2)replacement作为函数：  所传参数



re = /\d+(\D+)/
- $0 是母体，即是匹配成功的字符。   \d+(\D+)
- $1、$2、...、$99 与 regexp 中的第 1 到第 99 个子表达式相匹配的文本。   \D+
- $& 与 regexp 相匹配的子串。   //索引和整个字符串 代表的形参 依次往后推     
- $` 位于匹配子串左侧的文本。
- $' 位于匹配子串右侧的文本。
- $$ 直接量符号。



function($0,$1，$2){  //re = /\d+\D+/;
  $0:  匹配项
  $1:  索引
  $2： 整个字符串
  $3:  undifined，后面的参数都是未定义
}

--子表达式是用"()"括起来的匹配子项。

实例1：
```
    var name = "abcdsabcds";
    oText[0].value = name;
    oText[1].value = name.replace(/(a)b(c)/, "$2 $1");//b acdsabcds

 var json = {  把字符串中的{内容}替换成对象中的值
		name:'小明',
		age:18
	}
	
	var str = '今天{name}说:我今年{age}';
	
	var s = str.replace(/{[a-z]+}/g,function($0){  方法一
		
		return json[$0.replace(/{|}/g,'')];///^{|}$/g     /[{}]/g  
	});

	var s = str.replace(/{([a-z]+)}/g,function($0,$1){ 方法e二
		
		return json[$1];    
	});
	
	console.log(s);


```
<a href="正则7.html" target="_blank">实例</a>
其中(a)是第一个子项---$1;
而(c)是第二个子项---$2;
虽说$1和$2 是有name决定的但是这种情况下仍旧需要使用""来引入。

3.ECMAScript v3 规定，replace() 方法的参数 replacement 可以是函数而不是字符串。在这种情况下，每个匹配都调用该函数，它返回的字符串将作为替换文本使用。该函数的第一个参数是匹配模式的字符串。接下来的参数是与模式中的子表达式匹配的字符串，可以有 0 个或多个这样的参数。接下来的参数是一个整数，声明了匹配在 stringObject 中出现的位置。最后一个参数是 stringObject 本身。

实例2：
```
    var name = "abcdsabcds";
    oText[0].value = name;
    oText[1].value = name.replace(/(a)b(c)/, function($1,$2){
        return $1 + "这里是由函数添加的文字" + $2;
    });
```
<a href="正则8.html" target="_blank">实例</a>

实例3：
```
    var name = "Doe, John";
    oText[0].value = name;
    oText[1].value = name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");
```
<a href="正则9.html" target="_blank">实例</a>
`/(\w+)\s*, \s*(\w+)/`是指：一个以单词字符后紧跟着多个或零个空格的字符+,+一个以零个或多个空格开头并以单词字符结尾的字符。

实例4：
```
    var name = "Doe, John";
    oText[0].value = name;
    oText[1].value = name.replace(/o|e/g, "v");
```
<a href="正则10.html" target="_blank">实例</a>
"|"是代表"或"
`replace(/o|e/g, "v")`是指将所有的"o"或"e"都替换成"v";


##建议：
在经常使用正则下，可以将其与json配合使用：
比如：
```
        var re = {
            chinese:/[\u4e00-\u9fa5]/,
            space:/^\s*|\s*$/,
            Email:/^\w+@[a-z0-9]+(\.[a-z]+){1,3}$/,
            http:/[a-zA-z]+:[^\s]*/,
            qq:/[1-9][0-9]{4,9}/,
            locEmall:/[1-9]\d{5}/,
            id:/[1-9]\d{14}|[1-9]\d{17}|[1-9]\d{16}x/
        }
        oBtn.onclick = function(){
            oText[1].value = re.chinese.test(oText[0].value);
        }
```
<a href="正则12.html" target="_blank">实例</a>


#注：
1.如果要使用传参来建立正则，就要使用创建 RegExp 对象的语法：
`var re = new RegExp(pattern, attributes);`因为 如果使用直接量语法无法分辨变量和字符串。
2.正则中的贪婪和非贪婪特性：

http://www.jb51.net/article/31491.htm
简介：
>网上找到的贪婪与非贪婪模式详解，看了这一段基本明白贪婪与非贪婪模式的构成条件
1 概述
贪婪与非贪婪模式影响的是被量词修饰的子表达式的匹配行为，贪婪模式在整个表达式匹配成功的前提下，尽可能多的匹配，而非贪婪模式在整个表达式匹配成功的前提下，尽可能少的匹配。非贪婪模式只被部分NFA引擎所支持。
属于贪婪模式的量词，也叫做匹配优先量词，包括：
“{m,n}”、“{m,}”、“?”、“*”和“+”。
在一些使用NFA引擎的语言中，在匹配优先量词后加上“?”，即变成属于非贪婪模式的量词，也叫做忽略优先量词，包括：
“{m,n}?”、“{m,}?”、“??”、“*?”和“+?”。
从正则语法的角度来讲，被匹配优先量词修饰的子表达式使用的就是贪婪模式，如“(Expression)+”；被忽略优先量词修饰的子表达式使用的就是非贪婪模式，如“(Expression)+?”。
对于贪婪模式，各种文档的叫法基本一致，但是对于非贪婪模式，有的叫懒惰模式或惰性模式，有的叫勉强模式，其实叫什么无所谓，只要掌握原理和用法，能够运用自如也就是了。个人习惯使用贪婪与非贪婪的叫法，所以文中都会使用这种叫法进行介绍。 

~~~
正则匹配class
var re = new RegExp('(^|\\s)'+cName+'(\\s|$)','g');
以空格或非空格   开头、结尾  中间为匹配的class

var re = new RegExp('\\b'+ cName + '\\b','g') == new RegExp('^|\\s'+ 变量 + '\\s|$','g') 
cName两边都是单词边界。如果在字符串中有\必须转义。，否则会出错


/^[a-zA-Z]\w{5,17}@[a-z1-9]+.(com|com.cn|cn)$/  邮箱号
/^[\-\+]?(\d+)?(.\d{0，2})?$/.test(10.12) 有没有小数都可以
~~~




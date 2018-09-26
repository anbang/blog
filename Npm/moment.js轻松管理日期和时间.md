moment.js不依赖任何第三方库，支持字符串、Date、时间戳以及数组等格式，可以像PHP的date()函数一样，格式化日期时间，计算相对时间，获取特定时间后的日期时间等等，本文有如下举例。

https://zhubangbang.com/ssl/project/momentjs/index.html

格式化日期
当前时间：

moment().format('YYYY-MM-DD HH:mm:ss'); //2014-09-24 23:36:09 
今天是星期几：

moment().format('d'); //3 
转换当前时间的Unix时间戳：

moment().format('X'); 
相对时间
20120901相对当前日期是2年前

moment("20120901", "YYYYMMDD").fromNow(); //2 years ago 
7天后的日期：

moment().add('days',7).format('YYYY年MM月DD日'); //2014年10月01日 
9小时后的时间：

moment().add('hours',9).format('HH:mm:ss'); 
moment.js提供了丰富的说明文档，使用它还可以创建日历项目等复杂的日期时间应用。我们日常开发中最常用的是格式化时间，下面我把常用的格式制作成表格说明供有需要的朋友查看：


``` 
格式代码	说明	返回值例子
M	数字表示的月份，没有前导零	1到12
MM	数字表示的月份，有前导零	01到12
MMM	三个字母缩写表示的月份	Jan到Dec
MMMM	月份，完整的文本格式	January到December
Q	季度	1到4
D	月份中的第几天，没有前导零	1到31
DD	月份中的第几天，有前导零	01到31
d	星期中的第几天，数字表示	0到6，0表示周日，6表示周六
ddd	三个字母表示星期中的第几天	Sun到Sat
dddd	星期几，完整的星期文本	从Sunday到Saturday
w	年份中的第几周	如42：表示第42周
YYYY	四位数字完整表示的年份	如：2014 或 2000
YY	两位数字表示的年份	如：14 或 98
A	大写的AM PM	AM PM
a	小写的am pm	am pm
HH	小时，24小时制，有前导零	00到23
H	小时，24小时制，无前导零	0到23
hh	小时，12小时制，有前导零	00到12
h	小时，12小时制，无前导零	0到12
m	没有前导零的分钟数	0到59
mm	有前导零的分钟数	00到59
s	没有前导零的秒数	1到59
ss	有前导零的描述	01到59
X	Unix时间戳	1411572969
```

更多有关moment.js的介绍，看官网；

中文的翻译网站是：http://momentjs.cn/docs/
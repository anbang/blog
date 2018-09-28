### href的用法

- 1.内部连接：<a href=”#/URL”>name</a>
- 2.锚记：<a name=”object-name”>name</a><a href=”#object-name”>name</a>
- 3.外部链接：<a href=”URL”>name</a>
- 英文全称是 hypertext reference 表示一种超链接 ， （超文本引用）
- 比如：<a href=”http://www.google .com”>Google</a>
- 这句话就表示建立一个以“Google”（字）为表象的网址链接。
- 4.链接说明文字：<a href=”/” title=”链接说明”>链接说明</a>
- 5.特效链接

特效链接的目的不是跳转到其他位置，而是为了实现基本种页面特效，这种链接需要脚本来支持。

例：

JavaScript脚本：`<a href=”javascript:alert(‘夜深了早点休息吧！’)”>点击我！</a>`

`<a href=”javascript:;”>回到顶部</a>`通常用于跳转，且不跳转到某锚点#xxx，用来实现返回顶部等效果

VBScript脚本：`<a href=vbscript:msgbox(“现在时间是：”&time)>点击我！</a>`

6.诡异无名超链接

在HTML中，页面相互嵌套,再带上frame的总和应用时，超链接的路径错误是个非常令人头疼的问题。如在java web开发时，我就碰到这问题，下面是普通的超链接:`<a href=”findallsupplier.action?sign=0″></a>`经常报找不到文件或是路径中有重复路径存在！

解决方法：`<a href=”../../findallsupplier.action?sign=0″></a>`

诡异分析：诡异之处在于它不仅要指明这个超链接要去访问谁，还要指明服务器处理完再次跳转时的相对路径！

7.外部CSS引用：`<link type=”text/css” rel=”stylesheet” href=”../css/test.css” />`

8.如果`<a>链接</a>`不设置href的话部分浏览器将不会出现cursor“手指针”;

超链接的 URL。可能的值：

- 绝对 URL – 指向另一个站点（比如 href=”http://www.example.com/index.htm”）
- 相对 URL – 指向站点内的某个文件（href=”index.htm”）
- 锚 URL – 指向页面中的锚（href=”#top”）
- href：Hypertext Reference的缩写。意思是超文本引用。href 属性的值可以是任何有效文档的相对或绝对 URL，包括片段标识符和 JavaScript 代码段。如果用户选择了 <a> 标签中的内容，那么浏览器会尝试检索并显示 href 属性指定的 URL 所表示的文档，或者执行 JavaScript 表达式、方法和函数的列表。

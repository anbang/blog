parseInt 会先挪用 toString 方法；
```
alert(parseInt(0.000001));//0
alert(parseInt(0.0000001));//1

```
开始以为都是0；结果弹出来后，发现是0和1；

 

网上搜了下原因；是因为parseInt执行的时候会先挪用 toString 的方法；

```
alert(parseInt(0.000001));//0
alert(parseInt(0.0000001));//1
alert(parseInt("0.000001"));//0
alert(parseInt("0.0000001"));//0
```

下面两个才是我们真正想要的；

查看 ECMA-262 规范，parseInt 会先挪用 toString 方法。问题已逐步清楚：
```
alert(0.000001); alert(0.0000001);
```

第一条语句原样输出；

第二条语句输出 1e-7.
继续翻查 ECMA-262 9.8.1 ToString Applied to the Number Type 一节，

：
```
assertEquals(“0.00001”, (0.00001).toString()); assertEquals(“0.000001”, (0.000001).toString()); assertEquals(“1e-7”, (0.0000001).toString()); assertEquals(“1.2e-7”, (0.00000012).toString()); assertEquals(“1.23e-7”, (0.000000123).toString()); assertEquals(“1e-8”, (0.00000001).toString()); assertEquals(“1.2e-8”, (0.000000012).toString());

```
上面是 V8 引擎number-tostring 的单位测试剧本, 很好地诠释了 ECMA 规范。
小结：对小于 1e-6 的数值来讲，ToString 时会主动转换为科学计数法。 是以 parseInt 方式，在参数类型不确按时，最好封装一层：

```
function parseInt2(a) {
    if(typeof a === 'number') {
        return Math.floor(a);
    }
    return parseInt(a);
}
```

————————

下面是网上别人分享的思路；【实测正面是错误了】

js parseInt的陷阱分析小结,当第一个字符为0时，Js会把它看成一个8进制数字，其他8进制之外的字符都回被忽略掉。

```
var a = parseInt("09"), b = Number("09");
alert(a);
alert(b);
```

很多人会认为a和b的值都是数字9，但实际上不是。

【实测，弹出来的是9和9；】
parseInt的主要作用是把字符串转换为整数，或者把小数转换为整数。一般情况下，我们只用到它的第一个参数。但实际上，它有两个参数：
- parseInt(string, radix)
- parseInt会根据radix指定的进制进行转换，比如：

```
alert(parseInt("10", 2)); // outputs '2'//实测弹出的是2；
```

在没有指定radix或者radix为0的情况下，parseInt会按十进制进行转换。然而，这在某些情况下有点特殊：
* 如果string的值以“0x”开头，parseInt会按十六进制进行转换；
* 如果string的值以“0”开头，parseInt会按八进制进行转换。
说回开头的代码，由于”09″是以“0”开头，所以parseInt会按八进制进行转换，但是“9”不是合法的八进制值（八进制只有0－7八个数字），所以转换结果是0。

要避免这个陷进，可以强制指定radix：

```
alert(parseInt("09", 10)); // outputs '9'//实测弹出来的是9；
```
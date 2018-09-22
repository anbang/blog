取出某一个对象数组中，某一个key最大值所在的item；如果最大值有两个，则取索引靠前的那个item；
因为有很多条数据，而且频繁获取，需要性能做到最好

```javascript
var obj = [
    { "price": 11.00089, "number": 5.0179, "heji": 123.980 },
    { "price": 0.189, "number": 5.01, "heji": 1 },
    { "price": 0.00009, "number": 1, "heji": 10 },
    { "price": 0.00008, "number": 2, "heji": 10 },
    { "price": 0.00009101, "number": 15.01, "heji": 123.980 },
    { "price": 0.00109102, "number": 15.01, "heji": 3.08 }
];
```

需要的结果是

`{ "price": 0.00009101, "number": 15.01, "heji": 123.980 }`

核心代码如下

```javascript
function filterMaxItem(data,key) {
    var tempObj={};
    var maxVal=0;
    data.forEach(function (item, index) {
        var currentVal = item[key];
        if(currentVal>maxVal){
            maxVal=currentVal;
            tempObj=item;
        }
    });
    return tempObj;
}
console.log(filterMaxItem(obj,'number'));
```

原理是:假设mavVal当前值是最大值，如果有比maxVal还大的就替换，循环结束后，即可取到最大值和所在item；

利用是假设；
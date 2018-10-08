表格排序是常用的一个应用；

汉语的话需要用到localeCompare代替sort来比较；


var str1="朱",
    str2="嗷";
alert(str2.localeCompare(str1));//-1;判断汉字前后顺序的方法；

~

代码如下：
```html
<!doctype html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        body{
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;}
        table,td,tr,th{border-collapse: collapse;border: 1px solid orange;}
        th{cursor: pointer;}
    </style>
</head>
<body>
<table width="450" border="1" align="center">
    <thead>
        <th>姓名</th>
        <th>数学</th>
        <th>语文</th>
        <th>英语</th>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>34</td>
            <td>23</td>
            <td>84</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>47</td>
            <td>37</td>
            <td>62</td>
        </tr>
        <tr>
            <td>王五</td>
            <td>63</td>
            <td>35</td>
            <td>48</td>
        </tr>
        <tr>
            <td>李六</td>
            <td>34</td>
            <td>45</td>
            <td>19</td>
        </tr>
        <tr>
            <td>朱八</td>
            <td>64</td>
            <td>34</td>
            <td>67</td>
        </tr>
        <tr>
            <td>孙七</td>
            <td>45</td>
            <td>46</td>
            <td>24</td>
        </tr>
    </tbody>
</table>
</body>
</html>
<script>
    var oTable=document.getElementsByTagName("table")[0],
            oThead=oTable.tHead,
            oTbody=oTable.tBodies[0],
            oRows=oTbody.rows;
    var aRows=listToArray(oRows);
    var oThs=oThead.getElementsByTagName("th");
    for(var i=0;i<oThs.length;i++){
        (function(i){
            oThs[i].onclick= function () {
                rowSort(i,this);
            }
        })(i)
    };
    //rows这个是行集合，cells是行对象中的列；
    var flag=-1;
    function rowSort(i,key){
        //flag*=-1;
        if(key.flag){
            key.flag*=-1;
        }else{
            key.flag=1;//第一次,则把flag属性舒适化为1；这样可以确保每列点第一次都是升序排列；
        }
        aRows.sort(function(a,b){
            if(parseInt(a.cells[i].innerHTML)){
                return key.flag*(a.cells[i].innerHTML-b.cells[i].innerHTML);
            }else{
                return key.flag*(a.cells[i].innerHTML.localeCompare(b.cells[i].innerHTML));
            }
        });
        for(var n=0;n<aRows.length;n++){//添加到oTbody；
            oTbody.appendChild(aRows[n])
        };
    };
    function listToArray(eles) {
        if(!(typeof eles=="object"&&eles.length&&eles[eles.length-1])){
            throw new Error("不能操作非法的类数组");
        }
        var ary=[];
        try{
            ary=[].slice.call(eles,0);
        }catch (e){
            for(var i= 0,len=eles.length;i<len;i++){
                ary.push(this[i]);
            }
        }
        return ary;
    }
</script>
```
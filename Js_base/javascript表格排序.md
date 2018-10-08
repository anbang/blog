实现功能如下： 

- 1、写静态的html和css样式，html和CSS要分离；
- 2、拼一个json格式的对象(编号、姓名、年龄、邮箱、手机、工资) 5条；
- 3、数据绑定：把json格式的数据，绑定到我们的html页面中(文档碎片的方式)，实现数据和html分离
- 4、实现隔行变色，js和html分离；
- 5、实现表格排序:

    - 获取所有的行
    - 将DOM集合转化为数组
    - 数组按照每一行中的指定列进行排序(要区分数字还是非数字)
    - 在把排序后的数组 重新的绑定到页面上
    - 1)所有鼠标滑上去有小手效果的，都要进行排序
    - 2)第一次点击实现升序，再点击是降序，在点击又是升序…

- 6、实现全选和非全选:
          - 1)点击最上面的复选框，如果当前复选框选中，下面所有的都选中….
          - 2)在手动点击下面复选框的时候，也要根据是否都选中来改变我们表头的那个复选框的选中状

html代码如下：
```
    <!DOCTYPE html>
    <html>
    <head lang="en">
        <meta charset="UTF-8">
        <title></title>
        <link rel="stylesheet" type="text/css" href="css/table.css"/>
    </head>
    <body>
    <div id="div1">
        <table id="tab" cellpadding="0"     cellspacing="0" >
            <thead>
                <tr>
                    <th class="w30">
                        <input type="checkbox"/>
                    </th>
                    <th class="w70" id="name">姓名</th>
                    <th class="w100 cursor" id="age">年龄</th>
                    <th class="w100" id="mail">邮箱</th>
                    <th class="w200" id="phone">手机</th>
                    <th class="w200 cursor" id="score">工资</th>
                </tr>
            </thead>
            <tbody>
            </tbody>
        </table>
    </div>
    <script type="text/javascript" src="js/jsonPerson.js"></script>
    <!--json这个数据一定要在table.js前面，否则可能会出现报错undefined的情况-->
    <script type="text/javascript" src="js/tools.js"></script>
    <script type="text/javascript" src="js/table.js"></script>
    </body>
    </html>
```
CSS代码如下：

```
    html, body, div, input, table, tr, td, th{
        margin:0;
        padding: 0;
        font-size: 12px;
        font-family:  "微软雅黑", "宋体";
    }
    ul,li,td,th{
        list-style: none;}
    #div1{
        width: 700px;
        margin: 50px auto 0;
        padding: 5px;
        border: 1px solid #008CD7;
        border-radius: 5px;
    }
    thead tr,tbody tr{
        height: 30px;
        line-height: 30px;
        text-align: center;
        -webkit-user-select: none;
    }
    thead{
        height: 30px;
        background: #008CD7;
        cursor: pointer;
    }
    .w30 {   width: 30px;}
    .w70{width: 70px;}
    .w100{  width: 100px;}
    .w200{  width: 200px;}
    .c{background: #cccccc}
    .cy{background: #CDE074}

```
数据代码如下：

```
    var jsonData = [
        {
            name: "朱安邦",
            age: 25,
            email: "1234567890@qq.com",
            phone: "13245637823",
            score: 1340
        },
        {
            name: "刘安邦",
            age: 23,
            email: "4123456780@qq.com",
            phone: "18723456423",
            score: 1200
        },
        {
            name: "李安邦",
            age: 32,
            email: "4123456890@qq.com",
            phone: "13800026574",
            score: 1800
        },
        {
            name: "王安邦",
            age: 41,
            email: "5234567890@qq.com",
            phone: "13800993302",
            score: 1034
        },
        {
            name: "甄安邦",
            age: 40,
            email: "2234567890@qq.com",
            phone: "15802193302",
            score: 1456
        },
        {
            name: "爱安邦",
            age: 34,
            email: "2234567890@qq.com",
            phone: "13411293302",
            score: 2568
        },
        {
            name: "殷安邦",
            age: 23,
            email: "1323467890@qq.com",
            phone: "17723493302",
            score: 1445
        },
        {
            name: "康安邦",
            age: 34,
            email: "4723467890@qq.com",
            phone: "13833493302",
            score: 1740
        },
        {
            name: "邓安邦",
            age: 43,
            email: "9123456890@qq.com",
            phone: "15922493302",
            score: 1890
        },
        {
            name: "朱一名",
            age: 25,
            email: "9123456890@qq.com",
            phone: "13846379824",
            score: 1190
        },
        {
            name: "朱二名",
            age: 28,
            email: "9123456890@qq.com",
            phone: "15934493302",
            score: 1290
        },
        {
            name: "朱三名",
            age: 28,
            email: "9123456890@qq.com",
            phone: "15956783302",
            score: 1490
        },
        {
            name: "朱四名",
            age: 38,
            email: "9123456890@qq.com",
            phone: "15922496538",
            score: 1990
        },
        {
            name: "朱五名",
            age: 48,
            email: "9123456890@qq.com",
            phone: "15922497890",
            score: 1290
        },
        {
            name: "朱六名",
            age: 37,
            email: "9123456890@qq.com",
            phone: "15922434562",
            score: 2890
        },
        {
            name: "朱七名",
            age: 25,
            email: "9123456890@qq.com",
            phone: "15922474529",
            score: 3790
        },
        {
            name: "朱八名",
            age: 26,
            email: "9123456890@qq.com",
            phone: "15922499457",
            score: 2390
        },
        {
            name: "朱九名",
            age: 34,
            email: "9123456890@qq.com",
            phone: "15922492536",
            score: 1290
        },
        {
            name: "朱十名",
            age: 23,
            email: "9123456890@qq.com",
            phone: "15922492648",
            score: 2890
        },
    ];

```
tools.JS代码如下，其中用的是likeToArray这个方法：

```
    /*
     * tools(v1.0)：The project's toolkit, which contains the common methods of our project
     * by Team on 2015/06/04
     */
    var tools = {
        /*
         * formatJSON：Convert the JSON format to an JSON format object, compatible with all browsers
         * @parameter：jsonStr
         * @return：jsonObj
         */
        formatJSON: function (jsonStr) {
            var jsonObj = null;
            try {
                jsonObj = JSON.parse(jsonStr);
            } catch (e) {
                jsonObj = eval("(" + jsonStr + ")");
            }
            return jsonObj;
        },
        /*
         * isType:Detection data type
         * @parameter:
         *    val: value
         *    type: "String"、"Array"....
         * @return:true/false
         */
        isType: function (val, type) {
            return Object.prototype.toString.call(val) === "[object " + type + "]";
        },
        /*
         * likeToArray:Convert an array of classes into an array
         * @parameter:likeAry
         * @return:array
         */
        likeToArray: function (likeAry) {
            var ary = [];
            try {
                ary = [].slice.call(likeAry, 0);
            } catch (e) {
                for (var i = 0; i < likeAry.length; i++) {
                    ary.push(likeAry[i]);
                }
            }
            return ary;
        }
    };

```
下面是主角table.js的代码：

```
    var oTab=document.getElementById("tab");
    var oBody=oTab.tBodies[0];
    var oRows=oBody.rows;
     
    var tables={
        /*下面是一个封装函数，使表格变色的*/
        changeColor:function () {
            for (var i = 0; i < oRows.length; i++) {
                var oRow = oRows[i];
                i % 2 == 0 ? oRow.className = "c" : oRow.className="";
     
                oRow.oldClass = oRow.className;
                oRow.onmouseover = function () {
                    this.className = "cy";
                };
                oRow.onmouseout = function () {
                    this.className = this.oldClass;
                }
            }
        },
        /*下面是表格引入json的*/
        show:function () {
            var str = "";
            if (jsonData && jsonData.length > 0) {
                for (var i = 0; i < jsonData.length; i++) {
                    var cur = jsonData[i];
                    str += "<tr>";
                    str += "<td><input type='checkbox' name='tabInput'/></td>";
                    for (var key in cur) {
                        str += "<td>" + cur[key] + "</td>";
                    }
                    str += "</tr>";
                }
            }
            oBody.innerHTML = str;
            tables.changeColor();
        },
     
    };
    tables.show();
    tables.changeColor();
     
    /*表格排序*/
    function sortRows(n) {
        var oRows = oBody.rows;
        var oRowsAry = tools.likeToArray(oRows);
        //给所有行对应的数组进行排序
        oRowsAry.sort(function (a, b) {
            var as = a.cells[n].innerHTML;
            var bs = b.cells[n].innerHTML;
            if(isNaN(as)){
                return as.localeCompare(bs);
            }else{
                return as - bs;
            }
        });
        //实现升降序处理
        if (this.flag === "asc") {
            oRowsAry.reverse();
            this.flag = "desc";
        } else {
            this.flag = "asc";
        }
        //按照最新的顺序，把我们的每一行重新的添加到页面中
        var frg = document.createDocumentFragment();
        for (var i = 0; i < oRowsAry.length; i++) {
            frg.appendChild(oRowsAry[i]);
        }
        oBody.appendChild(frg);
        tables.changeColor();
    }
     
    /*oName.onclick = function () {
     sortRows.call(this, 1);
     *//*    alert("这是一个彩蛋！你真幸运！发触发了这个秘密！可凭此页面截图，到附近黄金店免费领取300克的项链一条！领取方式：拿起就跑，越快越好。 领取后还可以赢得看守所七天食宿全包超值游， 还送精美炫酷手铐脚链一条，时尚囚衣套装，警车接送等， 领的越多惊喜越多， 前十名还可享受免费剃头, 前一百名还可与警犬嬉戏， 来宾均赠棍棒按摩，电击去死皮美容保健套装， 还等什么，赶快行动吧 ！");*//*
     };
     //tools.css-js.com 在线压缩代码；
     //开始打算给每个innerHTML加onclick事件的，但是发现用循环来做，会更加的简单；下面是废弃代码
     var oScore = document.getElementById("score");
     var oAge = document.getElementById("age");
     var oName=document.getElementById("name");
     var oPhone=document.getElementById("phone");
     var oMail=document.getElementById("mail");
     */
    var oThs=document.getElementsByTagName("th");
    for(var i=0;i<oThs.length;i++){
        oThs[i].zhu = i;
        oThs[i].onclick=function(){
            sortRows.call(this,this.zhu);
        }
    }
     
    //选中效果；
    var oChk = document.getElementsByTagName("input")[0];
    selectAll(0);
    oChk.onclick = function () {
        this.checked ? selectAll(1) : selectAll(0);
    };
    function selectAll(flag) {
        var oChk = document.getElementsByTagName("input");
        for (var i = 1; i < oChk.length; i++) {
            oChk[i].onclick = function () {
                !this.checked ? oChk[0].checked = "" : void 0;
                var tmpFlag = true;
                for(var i=1; i<oChk.length; i++){
                    if(!oChk[i].checked){
                        tmpFlag = false;
                        break;
                    }
                }
                if(tmpFlag){
                    oChk[0].checked = "checked";
                }
            };
            flag === 0 ? oChk[i].checked = "" : oChk[i].checked = "checked";
        }
    }
```
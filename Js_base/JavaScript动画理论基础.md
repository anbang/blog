如果当前起始的位置小于我们的目标值的话，我们需要往右走，如果当前起始的位置大于我们的目标值的话，往左走，相等的话不走 
临界点判断：不管方向，都需要做临界判断,为了保障临界点的准确性，需要在判断的时候加上我们的步长

动画中的几部分:–>tween动画库 
开始位置、结束位置  总距离=结束位置-开始位置 
总运动时间、定时器间隔时间  步长=(总距离/总运动时间)*定时器间隔时间 
运动的方式:匀速、减速、加速、变速


··

HTML+CSS部分如下：
``` 
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        body, div {
            margin: 0;
            padding: 0;
            font-family: "\5FAE\8F6F\96C5\9ED1", Helvetica, sans-serif;
            font-size: 14px;
            -webkit-user-select: none;
        }
        #box {
            position: absolute;
            top: 0;
            left: 0;
            width: 200px;
            height: 200px;
            background: #37C7D4;
            border-radius: 50%;
        }
        #btnLeft, #btnRight {
            position: absolute;
            top: 400px;
            left: 0;
            width: 50px;
            height: 30px;
            line-height: 30px;
            text-align: center;
            background: #ff0000;
            cursor: pointer;
            border-radius: 10px;
        }
        #btnLeft {
            left: 100px;
        }
        #btnRight {
            left: 200px;
        }
    </style>
</head>
<body>
<div id="box"></div>
<div id="btnLeft">左</div>
<div id="btnRight">右</div>
</body>
</html>
```

JavaScript部分代码如下：
```
<script type="text/javascript" src="js/tools.min.js"></script>
<script type="text/javascript">
    var tool = new Tool;
    var box = document.getElementById("box");
    var btnLeft = document.getElementById("btnLeft");
    var btnRight = document.getElementById("btnRight");
    var cW = document.documentElement.clientWidth || document.body.clientWidth;
    var tarR = cW - box.offsetWidth;
    var tarL = 0;
    btnLeft.onclick = function () {
        move(tarL);
    };
    btnRight.onclick = function () {
        move(tarR);
    };
    function move(target) {
        var start = tool.getCss("left", box);
        var count = start;
        _move();
        function _move() {
            clearTimeout(box.timer);
            box.timer = setTimeout(_move, 10);
            if (start < target) {//右
                count += 10;
                tool.setCss(box, "left", count);
                if (count + 10 >= target) {
                    tool.setCss(box, "left", target);
                    return;
                }
            } else if (start > target) {//左
                count -= 10;
                tool.setCss(box, "left", count);
                if (count - 10 <= target) {
                    tool.setCss(box, "left", target);
                    return;
                }
            } else {
                return;
            }
        }
    }
</script>
 ```
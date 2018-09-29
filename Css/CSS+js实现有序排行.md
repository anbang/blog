HTML代码如下：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>无序列表CSS3</title>
    <link rel="stylesheet"href="css/main.css"/>
</head>
<body>
<div class="wrap">
    <ol id="ol1" class="list">
        <!--<li><a href="" target="_blank"></a></li>-->
    </ol>
</div>
<script>
    var str = '';
    var data = [
                { url : 'javascript:;', title : '朱安邦朱安邦' },
                { url : 'javascript:;', title : '朱安邦' },
                { url : 'javascript:;', title : '朱安邦朱安邦' },
                { url : 'javascript:;', title : '朱安邦朱邦' },
                { url : 'javascript:;', title : '朱安朱安邦' },
                { url : 'javascript:;', title : '朱邦朱安邦' },
                { url : 'javascript:;', title : '朱安邦朱安邦安邦朱安邦' },
                { url : 'javascript:;', title : '朱安邦朱安安邦朱安邦' },
                { url : 'javascript:;', title : '朱安安邦朱安邦' },
                { url : 'javascript:;', title : '查看本页实现原理' }
    ];
    data.forEach(function(item){
        str+='<li><a href="'+item.url+'" target="_blank">'+item.title+'</a></li>'
    });
    document.getElementById('ol1').innerHTML = str;
</script>
</body>
</html>
```

CSS代码如下

```css
/*清楚样式*/
body,ol,div,li{  margin: 0;  padding: 0; font-size: 18px;font-family: "微软雅黑"; }
a {text-decoration: none;color: #101010}
a:hover {  text-decoration: none;}
/*正式部分*/
.wrap {  width: 500px;  font-size: 22px;  margin: 20px auto 0;}
.list {  padding: 0 60px;}
.list li {line-height: 50px;  border-bottom: 1px solid #CCC;  }
.list li a {display: block;padding: 5px 10px;  transition: all 0.6s;  }
.list li:nth-last-child(1) a{color: #ff0000;}
.list li a:hover {
    /*text-indent首行缩进的，效果的主要*/
    text-indent: 25px;
    border-radius: 5px;
    color: #fff;
    text-shadow: 0 2px 1px #360;
    background-color: #393;
    /*下面是四套渐变的，没写欧朋的*/
    background-image: -webkit-linear-gradient(top,#62c462,#51a351);
    background-image: -moz-linear-gradient(top,#62c462,#51a351);
    background-image: -ms-linear-gradient(top,#62c462,#51a351);
    background-image: linear-gradient(top,#62c462,#51a351);
}
```
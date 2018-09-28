一个渐变文字，

色彩还行，作为标题使用的；

![](./img/css3-be-01.png)

DEMO如下：

![](./img/css3-be-02.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .title{
            font-size: 68px;
            font-weight: normal;
            color: rgb(65, 133, 225);
            text-transform: uppercase;
            line-height: 1.2;
            text-align: center;
            background: -webkit-linear-gradient(left, #72e48d, #4c9ace);
            background: linear-gradient(left, #72e48d, #4c9ace);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
    </style>
</head>
<body>
<p class="title">这是一段渐变的文字</p>
</body>
</html>
```

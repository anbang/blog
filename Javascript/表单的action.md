表单是向服务器提交数据用的，比如用户注册；action=’’是提交到哪里的意思：

 

语法如下：
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<form   action="http://taobao.fm/">
    name <input type="text" name="user" /><br>
    pass <input type="password" name="pass"/>
    <input type="submit"/>
</form>
</body>
</html>
```
其中，submit是提交按钮的意思；
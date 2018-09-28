一个表单需要对多个项目赋值的时候，如果赋值，需要手动赋值，或者写一个循环；这样做虽然可以，但是会有很多重复的代码，而且效率低，不优雅；

通过jquery.formautofill这个插件，可以自动填充到输入框/选择项等的内；

填充方式有两种，一种是通过name值来填充，一种是通过ID来填充；

组装一个JSON格式的数据，
```
    //编辑抽奖信息;
    $page.on("click",".j-edit-prize",function(e){
        e.preventDefault();
        var sourcesDataObj=$(this).closest("tr").data();
        self.initPrizeInfo(sourcesDataObj); 
    });
```

数据格式如：

```
    var data = {
        "name": "name_value",
        "email": "email@xxx.com",
        "lovejquery": "yes" //匹配的是value是yes的那个选择按钮
    }
```

然后jquery获取表单后，使用autofill 方式即可；

```
    initPrizeInfo:function(dataObj){
        //初始化值
        $editPrizeDiaForm.autofill(dataObj); 
    },
```

默认的这种方式是通过name值来查找的，

autofill的参数：

```
    var options = {
        findbyname: true,//通过name来查找并赋值,如果是false，是通过ID的方式来查找
        restrict: true//是否限制在当前form内赋值，上下文是当前form；如果是false，上下文是整个文档；
    } 
    initPrizeInfo:function(dataObj){ 
        //初始化值 
        var options = { findbyname: true, restrict: true }
        $editPrizeDiaForm.autofill(dataObj,options ); 
    },
```

参考：

```
DEMO:http://labs.creative-area.net/jquery.formautofill/doc/ 
GitHub:https://github.com/creative-area/jQuery-form-autofill
```

~~~~~~
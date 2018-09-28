表单验证：jquery.validate，在做表单验证的时候经常使用；

参考网站：http://www.runoob.com/jquery/jquery-plugin-validate.html

参考DEMO：http://www.runoob.com/try/try.php?filename=jquery-plugin-errorcontainer

 

默认校验规则：

- required: true 值是必须的。
- required: “#aa:checked” 表达式的值为真，则需要验证。
- required: function(){} 返回为真，表示需要验证。 
- 后边两种常用于，表单中需要同时填或不填的元素。


校验的规则：

Debug
```
$helpUserReplaceDiaForm.validate({debug:true})//只验证，不提交

如果多个页面都需要调多个表单

$.validator.setDefaults({
    debug: true
})
```
备注：写模块的时候，这么用，发现设置Debug无效（表单提交是通过submitHandler 提交）

errorPlacement：更改错误信息显示的位置;

validate方法返回一个Validator对象，validator对象有很多方法可以用来引发校验程序或者改变form的内容；下面是常用的方法

### 验证函数DEMO如下:

```javascript
formValidate:function(){
    $helpUserReplaceDiaForm.validate({
    rules: {
        qrcode: {
            required: true
            },
        linkman: {
            required: true
        },
        mobile: {
            required: true,
        },
        district_code: {
            required: true
        },
        address: {
            required: true
        },
        reason: {
            required: true,
        }
    },
    messages: {
        qrcode: {
            required: "请填写或者扫描设备二维码"
        },
        linkman: {
            required: "请填写联系人"
        },
        mobile: {
            required: "请填写联系电话"
        },
        district_code: {
            required: "请选择所属区域"
        },
        address: {
            required: "请填写详细地址"
        },
        reason: {
            required: "请填写换货原因"
        }
    },
    errorPlacement: function (error, el) { //更改错误信息显示的位置处理
        var msg = error.html(),
            $p = el.closest('div.wui-form-item');
        if ($p.length == 0) {
            $p = el.closest('div.wui-form-itemother')
        }

        var $explain = $p.children('.wui-form-explain');
        if (msg !== '') {
            if ($explain.length == 0) {
                $explain = $('<p class="wui-form-explain wui-tiptext"></p>');
                $explain.appendTo($p);
            }
            $explain.html('<i class="iconfont">&#xe611;</i>' + msg);
            $p.addClass('wui-form-item-error');
        } else {
            $explain.remove();
            //$explain.html('');
            $p.removeClass('wui-form-item-error');
        }
    },
    success: function (label) { //成功
    },
    submitHandler: function (fm) { //验证通过后，保存数据
        console.log("验证通过");
    }
    })
},
```

### 自定义验证规则如下：

```javascript
    $.validator.addMethod("isQrcode", function (value, element) {
    var qrcode = $.trim(value);
        if (!qrcode) {
    return false;
        }
    });
```

可以做一些验证是否是URL，是否是手机号，是否身份证等验证；

- 表单填充：jquery.formautofill 
- 参考网站：https://github.com/creative-area/jQuery-form-autofill 
- 参考DEMO：http://labs.creative-area.net/jquery.formautofill/doc/

优点是，获取服务端返回的数据时候，可以自动识别name或者ID来填充数据；

如果是用户填写的数值，弹窗新窗口让用户确认数据的时候，这个时候并不适合用（如果用的话，需要在确认页面，对应项目设置ID，插件配置findbyname: false，获取用户输入的值，保存在对象中，对象的key值对应ID；）
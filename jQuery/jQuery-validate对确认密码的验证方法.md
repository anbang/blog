内部已经有API了，可以直接调用；不需要自己再用单独写；

API见官网：https://jqueryvalidation.org/equalTo-method/

使用如下

```
    pwd: {
        required: true,
        pwdNo: true
    },
    confirmPwd: {
        required: true,
        equalTo: "#pwd"
    }
```

`#pwd` 是对应密码区input的id；

这样就可以
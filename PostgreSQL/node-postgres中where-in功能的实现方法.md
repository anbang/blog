因为是在nodejs中使用的（基于node-postgres）

不能直接使用 where in 功能

## 错误的用法

比如在原生SQL中的下面语句

```postgresql
select account from accounts where account in 
('czr_3ijpzyoofukqrhjyqk5ma6c64m44fexc1idta7necszksbj6urfqq7yu3dsy','czr_1aptm6u579y1gx548hj7nmehtu3jmduxmp9pwm4abxyiartcd5kd4ofw1dyd')
```

转成where如下

```javascript
let accAry = ['czr_3ijpzyoofukqrhjyqk5ma6c64m44fexc1idta7necszksbj6urfqq7yu3dsy', 'czr_1aptm6u579y1gx548hj7nmehtu3jmduxmp9pwm4abxyiartcd5kd4ofw1dyd'];
let upsertSql = {
    text: "select account from accounts where account in ($1)",
    values: [accAry]
};
 
pgclient.query(upsertSql, (res) => {
    console.log("accounts", res)
});
```

但是并没有什么软用，这个获取不到想要的数据

## 正确的用法

改为使用 `ANY` 即可 ,代码如下

```javascript
let accAry = ['czr_3ijpzyoofukqrhjyqk5ma6c64m44fexc1idta7necszksbj6urfqq7yu3dsy', 'czr_1aptm6u579y1gx548hj7nmehtu3jmduxmp9pwm4abxyiartcd5kd4ofw1dyd'];
let upsertSql = {
    text: "select account from accounts where account = ANY ($1)",
    values: [accAry]
};
 
pgclient.query(upsertSql, (res) => {
    console.log("accounts", res)
});
```

这样就可以获取到数据了
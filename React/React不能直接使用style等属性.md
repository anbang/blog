## 提示的错误消息如下

`Uncaught Error: Minified React error #62; visit http://facebook.github.io/react/docs/error-decoder.html?invariant=62&args[]=%20This%20DOM%20node%20was%20rendered%20by%20%60ProductRow%60. for the full message or use the non-minified dev environment for full errors and additional helpful warnings.`

错误的出处

```javascript
const name=this.props.product.stocked ? this.props.product.name :
    <span style='color:"red"'>
        {this.props.product.name}
    </span>;
return(
    <tr>
        <td>{name}</td>
        <td>{this.props.product.price}</td>
    </tr>
)
```

其中color:red 是错误的原因；

正确的写法如下

```javascript
const name=this.props.product.stocked ? this.props.product.name :
    <span style={{color:"red"}}>
        {this.props.product.name}
    </span>;
return(
    <tr>
        <td>{name}</td>
        <td>{this.props.product.price}</td>
    </tr>
)
```

这样就OK了，作为表达式，不能直接字符串包着；


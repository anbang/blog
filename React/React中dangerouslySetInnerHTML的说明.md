问题相关：React 在保护你免受 [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting) 攻击，所以用 `dangerouslySetInnerHTML`；

在渲染React内容的时候；

因为需要借助 markdown 格式的数据,代码如下

```javascript
const Comment=React.createClass({
    rawMarkup:function () {
        const md=new Remarkable();
        const rawMarkup=md.render(this.props.children.toString());
        return {__html:rawMarkup}
    },
    render:function () {
        return (
            <div className="comment">
                <h2 className="commentAuthor">
                    {this.props.author}
                </h2>
                <span dangerouslySetInnerHTML={this.rawMarkup()} />
            </div>
        )
    }
});
```
                    
文章用到了 `dangerouslySetInnerHTML` 这个属性，

这是一个危险使用 `innerHtml` 的属性；

这是一个特殊的 API，故意让插入原始的 HTML 变得困难，

但是对于 remarkable 我们将利用这个后门。

注意：使用这个功能,你需要保证 `remarkable` 是安全的。

在彻底的理解安全问题后果并正确地净化数据之后，生成只包含唯一 key `__html` 的对象，并且对象的值是净化后的数据。

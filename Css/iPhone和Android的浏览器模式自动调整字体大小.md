```
    -webkit-text-size-adjust: 100%;
        -ms-text-size-adjust: 100%;
```
iPhone 和 Android 的浏览器纵向 (Portrate mode) 和橫向 (Landscape mode) 模式皆有自动调整字体大小的功能。控制它的就是 CSS 中的 -webkit-text-size-adjust。

text-size-adjust 设为 none 或者 100% 关闭字体大小自动调整功能.


这属性现在的一般用处是防止iPhone在坚屏转向横屏时放大文字（注意，就算viewport设置了maximum-scale=1.0 文字还是会放大的）。

而且iPhone和iPad的默认设定是不一样的，iPhone默认设定 `-webkit-text-size-adjust: auto;`

iPad默认设定 `-webkit-text-size-adjust: none;`所以iPad默认是不调节的。

此属性还支持百分比，这在当前的桌面版的webkit浏览器是不支持的，所以如果不想让iPhone横坚屏切换的时候调节文字，用 `-webkit-text-size-adjust: 100%; `

绝对不能用 ` -webkit-text-size-adjust: none;`这会导致仍然支持 `-webkit-text-size-adjust: none;`的桌面版的webkit浏览器无法人为放大文字大小，严重影响可用性。
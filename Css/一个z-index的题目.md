搜了下东西；

看到有这么一句话；
```
    当子层和父层都定义了z-index的属性的时候，z-index值以父层的z-index为准，这个是web前端应该谨记的一点，这个在实际切图中经常会碰到这个问题，下次在碰到这种问题就不会搞错了。

    ps：需要使用子层z-index属性的时候，那就要删除子层父层上的z-index属性，如果要用父层z-index属性，那么子层z-index不需要定义，因为定义也是无效的。
```
但是写的时候发现，好像z-index最大的还是显示完；

我是这么写的‘
```html
    <!DOCTYPE html>
    <html>
        <head lang=”en”>
            <meta charset=”UTF-8″>
            <title></title>
            <style>
                body{
                    position: relative;
                }
                .div1{
                    width: 500px;
                    height: 500px;
                    background: darkgreen;
                    z-index: 100;
                }
                .div2{
                    width: 200px;
                    height: 200px;
                    background: red;
                    position: relative;
                    top: 250px;
                    left: 250px;
                    z-index: 500;
                }
                .div3{
                    width: 200px;
                    height: 200px;
                    background: yellow;
                    position: relative;
                    top: -100px;
                    left: 400px;
                    z-index: 300;
                }
            </style>
        </head>
        <body>
            <div class=”div1″>
            <div class=”div2″></div>
            </div>
            <div class=”div3″></div>
        </body>
    </html>
```
定位无论用relative还是absolute，都是z-index的最大；
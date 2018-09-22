页面中默认的样式比较丑；

![](./img/scroll1.png)

可以美化下；

美化后的滚动条如下

![](./img/scroll2.png)

CSS代码如下

```css
.cnc-index .team-wrap .team-item{
    width: 430px;
    height: 170px;
    overflow-x: hidden;
    overflow-y: auto;
    background-color: #303050;border-radius: 4px;
    padding: 30px;
    color: #fff;
    margin-left: 10px;
    margin-bottom: 10px;
    display: -webkit-flex;  display: flex;
    position: relative;
    cursor: pointer;
}
 
/*滚动条样式*/
.team-item::-webkit-scrollbar {/*滚动条整体样式*/
    width: 6px;     /*高宽分别对应横竖滚动条的尺寸*/
    height: 2px;
}
.team-item::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
    border-radius: 5px;
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    background: rgba(255,255,255,0.2);
}
.team-item::-webkit-scrollbar-track {/*滚动条里面轨道*/
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    border-radius: 0;
    background: rgba(0,0,0,0.1);
}
```
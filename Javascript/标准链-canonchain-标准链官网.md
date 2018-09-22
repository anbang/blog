标准链官网：

[官网](http://www.canonchain.com/) 

[新闻页](http://www.canonchain.com/news)

设计预览在站酷：[标准链官网设计稿](https://www.zcool.com.cn/work/ZMjY4MjMwNzI=.html)

兼容：PC端/手机端/Pad端、中英文双语版

技术要点：

- 粒子动画
- 滚动条自定义

**粒子动画**：使用 particles做的首屏粒子动画；

因为PC和phone的粒子数量不同，所以根据userAgent来定义粒子数量

**滚动条样式**：重写了CSS

```javascript
/*滚动条样式*/
.item-des-wrap::-webkit-scrollbar {/*滚动条整体样式*/
    width: 8px; /*高宽分别对应横竖滚动条的尺寸*/
    height: 2px;
}
.item-des-wrap::-webkit-scrollbar-track {/*滚动条里面轨道*/
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.1);
    border-radius: 0;
    background: rgba(255,255,255,1);
}
.item-des-wrap::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
    border-radius: 5px;
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    background: rgba(255,255,255,0.2);
}
```

**踩到的坑**：safari对flex兼容不好（一行item，设置flex属性无法自动换行，chrome/火狐没问题）！！！！！

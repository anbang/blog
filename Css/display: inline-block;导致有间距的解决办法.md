display: inline-block;会导致元素有4px空白间距

如图；

![](./img/inline-block-01.png)

解决办法：

1、去掉标签之间的空格；

![](./img/inline-block-02.png)


或者用注释也可以

![](./img/inline-block-03.png)


2、用font-size来解决

父级添加font-size:0；然后本身再把font-size恢复下即可；

![](./img/inline-block-04.png)



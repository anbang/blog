vue在取多级数据的时候，会有一个错误，

如果是 这样的值

movie_data.subject_collection.name

```
[Vue warn]: Error in render function: “TypeError: Cannot read property ‘name’ of undefined”
```
问题描述：

核心代码：

`<h2 v-text="module_title || movie_data.subject_collection.name"></h2>`


当module_title为空字符串的时候，取值 `movie_data.subject_collection.name`；虽然可以正常渲染出来，但是会抛一个警告说 name是undefined；

分析的原因是：

取一级没问题的， 比如movie_data.aaa 这种的一级没有问题，但是二级的这种，movie_data.aaa.bbb就会警告 bbb是undefined（只是警告，不报错，渲染还是正常渲染的）；

解决办法：

先判断上一级是否存在，如果存在再取

``` 
<h2 v-text="module_title || (movie_data.subject_collection && movie_data.subject_collection.name)"></h2>
```

这样就ok了

简单的一句话：判断上一级数据是否存在即可
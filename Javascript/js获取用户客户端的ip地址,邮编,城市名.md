pc端项目，会遇到一些需求，需要拿到用户的ip地址，id，城市名这些信息；

场景：根据用户的地址，选择收货地址的时候，可以直接定位到当前省市，方便用户体验 ；

推荐第三方使用：
```
http://pv.sohu.com/cityjson?ie=utf-8
```
使用的方法很简单：
```html
<script src="http://pv.sohu.com/cityjson?ie=utf-8"></script>
<script>
    console.log(returnCitySN);//returnCitySN 就是用户的ip相关信息；
    console.log(returnCitySN.cid);//330100
    console.log(returnCitySN.cip);//183.128.165.1xx
    console.log(returnCitySN.cname);//浙江省杭州市
</script>
```
这样就获得了用户ip等信息；

我一般是用来做，根据用户地址，推荐用户周边的我们服务；

或者根据用户地址，自动获取省市。写地址等时候，方便体验的;

～其它相关的～

其它的IP查询，不过有些已经失效了 在这总结下
```
    腾讯，pconline 的API已经失效 不能使用
    淘宝的IP接口地址: http://ip.taobao.com/instructions.php 
    腾讯的IP地址API接口地址：http://fw.qq.com/ipaddress
    新浪的IP地址查询接口：http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js
    新浪多地域测试方法：http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js&ip=183.128.165.140
    搜狐IP地址查询接口（默认GBK）：http://pv.sohu.com/cityjson
    搜狐IP地址查询接口（可设置编码）：http://pv.sohu.com/cityjson?ie=utf-8
```
搜狐另外的IP地址查询接口：
```
http://txt.go.sohu.com/ip/soip 
```
这个上面大多都容易查到

还有一个 API比较全面
```
http://whois.pconline.com.cn
```
这个很强大 也比较详细

但是这个有问题 他JSON格式 属于回调  本地运行可以 放到项目里面就报错403；
```
http://ip.taobao.com/instructions.php
```
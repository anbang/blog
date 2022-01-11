百度图标，自定义图标的；

```javascript
var map = new BMap.Map("map"); // 创建地图实例     
var point = new BMap.Point(116.404, 39.915); // 创建点坐标     
map.centerAndZoom(point, 15); // 初始化地图，设置中心点坐标和地图级别    
 
map.addControl(new BMap.NavigationControl());
map.addControl(new BMap.ScaleControl());
map.setDefaultCursor("crosshair");
map.addEventListener("click", function(e){   //点击事件    
    if(!e.overlay){
        var myIcon = new BMap.Icon("http://api.map.baidu.com/img/markers.png", new BMap.Size(23, 25), {
            offset: new BMap.Size(10, 25), // 指定定位位置  
            imageOffset: new BMap.Size(0, 0 - 10 * 25) // 设置图片偏移  
        });
        var marker=new BMap.Marker(e.point,{icon:myIcon});
        map.removeOverlay(preMarker);
        map.addOverlay(marker);
        preMarker=marker;
    }
});  
```

逻辑如上;

高德地图的参考这个：

http://lbs.amap.com/api/indoormap-js-api/guide/else-function/basic-types/
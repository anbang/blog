需要服务器允许跨域；

如果对方设置了防盗链；

则会遇到跨域的问题；

```javascript
var oImg=document.getElementById("target");
var testUrl="http://dl2.iteye.com/upload/attachment/0119/9050/985bb28a-0ce6-36df-83ac-0a6781c9691a.png";
    
function convertImgToBase64(url, callback, outputFormat){
    var canvas = document.createElement('canvas'),
        ctx = canvas.getContext('2d'),
        img = new Image;
    img.crossOrigin = 'Anonymous';//它开启了本地的跨域允许。当然服务器存储那边也要开放相应的权限才行，如果是设置了防盗链的图片在服务端就没有相应的权限的话你本地端开启了权限也是没有用的
    img.onload = function(){
        canvas.height = img.height;
        canvas.width = img.width;
        ctx.drawImage(img,0,0);
        var dataURL = canvas.toDataURL(outputFormat || 'image/png');//没权限的跨域图片在使用canvas.toDataURL()数据导出时会报错
        callback.call(this, dataURL);
        canvas = null;
    };
    img.src = url;
}
    
convertImgToBase64(testUrl, function(base64Img){
    console.log(base64Img);
    oImg.setAttribute("src",base64Img)
});
```
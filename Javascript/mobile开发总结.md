转的

## 1、隐藏地址栏
很多文档介绍通过调用
``` 
window.scrollTo(0, 1);
```

就可以隐藏地址栏，但是通过实践发现隐藏地址栏还是真够坑爹的啊，只调用这一句话一般不会起作用，我们需要
```
function hideBar() { setTimeout( function() { window.scrollTo(0, 1); }, 0); };
```
但是有时候我们发现还是隐藏不了地址栏，为什么呢？大多时候是因为页面高度不够隐藏地址栏，这样我们需要先把body高度设置够搞，隐藏地址栏后再还原回来，

```
 function hideBar() { document.body.style.height = Math.max(windows.innerHeight, windows.innerWidth) * 2 + 'px'; setTimeout( function() { window.scrollTo(0, 1); setTimeout( function() { document.body.style.height = windows.innerHeight + 'px'; }, 200); }, 0); };
 ```
 
好了这回终于可以隐藏地址栏了，但是我们又发现获取的innerHeight是隐藏地址栏之前的数值，怎么办呢，嗯，看来还需要一个回调函数，这样我们的方法又变成，
```
function hideBar(fn) { document.body.style.height = Math.max(windows.innerHeight, windows.innerWidth) * 2 + 'px'; setTimeout( function() { window.scrollTo(0, 1); setTimeout( function() { document.body.style.height = windows.innerHeight + 'px';    
```          

在实际应用的时候会发现有些机器反应比较慢，这样setTimeout的时间也可以加长些，唉，隐藏个地址栏竟然也这么麻烦， Tangram Mobile中已经实现了hideBar方法，还是建议大家直接使用baidu.page.hideBar吧。有些时候需要判断当前是否已经隐藏了地址栏，可以使用window.pageYOffset来判断。

## 2、获取设备翻转状态
我们可以通过获取orientation的值来判断翻转状态，那如果设备不支持orientation怎么样，那我们可以通过 innerWidth和innerHeight的比例来判断翻转状态，代码如下（取自Tangram Mobile的getOrientation），
```
baidu.page.getOrientation = function() { if ("onorientationchange" in window) { return (window.orientation == 0 || window.orientation == 180) ? 'portrait' : 'landscape'; } else { return (windows.innerHeight > windows.innerWidth) ? 'portrait' : 'landscape'; } };
```
## 3、position:fixed
在pc上我们经常使用position:fixed，在iphone上似乎并不管用，官方给了很多解释，但是它还是不好使，这样我们只能自己来实现这种效果，首先我们需要一个setPosition方法，
```
function setPosition(top, left){ //根据top、left把元素设置到视图区相应位置 }
```
然后，我们需要注册scroll事件和onrientation事件
```
element.addEventListener('scroll', setPosition); element.addEventListener('onrientationchange', setPosition);
```
具体代码可以参考Tangram Mobile的Toolbar组件

## 4、overflow:scroll
在iphone上开发web程序，overflow:scroll十分让人头疼，因为iphone并不支持，官方依然给了很多解释，我们还是需要自己来实现，因为不支持overflow:scroll，我们只能设置为overflow:hidden，然后在内容上注册scroll和touch事件，当touch事件被触发的时候，把隐藏的内容显示出来。这个功能比较复杂，我们可以使用Tangram Mobile的Scroller组件。

注：ios5已经解决了position:fixed和overflow:scroll的问题

#### WebApp与Native App有何区别呢？

Native App：

- 开发成本非常大一般使用的开发语言为JAVA、C++、Objective-C
- 更新体验较差、同时也比较麻烦每一次发布新的版本，都需要做版本打包，且需要用户手动更新（有些应用程序即使不需要用户手动更新，但是也需要有一个恶心的提示）。
- 非常酷因为native app可以调用IOS中的UI控件以UI方法，它可以实现WebApp无法实现的一些非常酷的交互效果
- Native app是被Apple认可的Native app可以被Apple认可为一款可信任的独立软件，可以放在Apple Stroe出售，但是Web app却不行。

Web App：

- 开发成本较低使用web开发技术就可以轻松的完成web app的开发
- 升级较简单升级不需要通知用户，在服务端更新文件即可，用户完全没有感觉
- 维护比较轻松和一般的web一样，维护比较简单，它其实就是一个站点

Webapp说白了就是一个针对Iphone、Android优化后的web站点，它使用的技术无非就是HTML或HTML5、CSS3、JavaScript，服务端技术JAVA、PHP、ASP。

当然，因为这些高端智能手机（Iphone、Android）的内置浏览器都是基于webkit内核的，所以在开发WEBAPP时，多数都是使用HTML5和CSS3技术做UI布局。当使用HTML5和CSS3l做UI时，若还是遵循着一般web开发中使用HTML4和CSS2那样的开发方式的话，这也就失去了WEBAPP的本质意义了，且有些效果也无法实现的，所以在此又回到了我们的主题–webapp的布局方式和技术。

在此说明一下，在此所说的移动平台前端开发是指针对高端智能手机（如Iphone、Android）做站点适配也就是WebApp，并非是针对普通手机开发Wap 2.0，所以在阅读本篇文章以前，你需要对webkit内核的浏览器有一定的了解，你需要对HTML5和CSS3有一定的了解。如果你已经对此有所了解，那现在就开始往下阅读吧……

1、首先我们来看看webkit内核中的一些私有的meta标签，这些meta标签在开发webapp时起到非常重要的作用

```
<metacontent=”width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;”name=”viewport”/>
<metacontent=”yes”name=”apple-mobile-web-app-capable”/>
<metacontent=”black”name=”apple-mobile-web-app-status-bar-style”/>
<metacontent=”telephone=no”name=”format-detection”/>
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;" name="viewport" /><meta content="yes" name="apple-mobile-web-app-capable" /><meta content="black" name="apple-mobile-web-app-status-bar-style" /><meta content="telephone=no" name="format-detection" />
```

- 第一个meta标签表示：强制让文档的宽度与设备的宽度保持1:1，并且文档最大的宽度比例是1.0，且不允许用户点击屏幕放大浏览；
- 第二个meta标签是iphone设备中的safari私有meta标签，它表示：允许全屏模式浏览；
- 第三个meta标签也是iphone的私有标签，它指定的iphone中safari顶端的状态条的样式；
- 第四个meta标签表示：告诉设备忽略将页面中的数字识别为电话号码；

2、HTML5标签的使用

在开始编写webapp时，哥建议前端工程师使用HTML5，而放弃HTML4，因为HTML5可以实现一些HTML4中无法实现的丰富的WEB应用程序的体验，可以减少开发者很多的工作量，当然了你决定使用HTML5前，一定要对此非常熟悉，要知道HTML5的新标签的作用。比如定义一块内容或文章区域可使用section标签，定义导航条或选项卡可以直接使用nav标签等等。

3、放弃CSS float属性

在项目开发过程中可以会遇到内容排列排列显示的布局，假如你遇见这样的视觉稿，哥建议你放弃float，可以直接使用`display:block;`

4、利用CSS3边框背景属性

这个按钮有圆角效果，有内发光效果还有高光效果，这样的按钮使用CSS3写是无法写出来的，当然圆角可以使用CSS3来写，但高光和内发光却无法使用CSS3编写，这个时候你不妨使用-webkit-border-image来定义这个按钮的样式。-webkit-border-image就个很复杂的样式属性。

5、块级化a标签

请保证将每条数据都放在一个a标签中，为何这样做？因为在触控手机上，为提升用户体验，尽可能的保证用户的可点击区域较大。

6、自适应布局模式

在编写CSS时，我不建议前端工程师把容器（不管是外层容器还是内层）的宽度定死。为达到适配各种手持设备，我建议前端工程师使用自适应布局模式（支付宝采用了自适应布局模式），因为这样做可以让你的页面在ipad、itouch、ipod、iphone、android、web safarik、chrome都能够正常的显示，你无需再次考虑设备的分辨率。

7、学会使用webkit-box

上一节，我们说过自适应布局模式，有些同学可能会问：如何在移动设备上做到完全自适应呢？很感谢webkit为display属性提供了一个webkit-box的值，它可以帮助前端工程师做到盒子模型灵活控制。

8、如何去除Android平台中对邮箱地址的识别

看过iOS webapp API的同学都知道iOS提供了一个meta标签:用于禁用iOS对页面中电话号码的自动识别。在iOS中是不自动识别邮件地址的，但在Android平台，它会自动检测邮件地址，当用户touch到这个邮件地址时，Android会弹出一个框提示用户发送邮件，如果你不想Android自动识别页面中的邮件地址，你不妨加上这样一句meta标签在head中


```
<metacontent=”email=no”name=”format-detection”/>
<meta content="email=no" name="format-detection" />
```
9、如何去除iOS和Android中的输入URL的控件条

你的老板或者PD或者交互设计师可能会要求你：能否让我们的webapp更加像nativeapp，我不想让用户看见那个输入url的控件条？ 答案是可以做到的。我们可以利用一句简单的javascript代码来实现这个效果

```

setTimeout(scrollTo,0,0,0);
setTimeout(scrollTo,0,0,0);
```
请注意，这句代码必须放在`window.onload`里才能够正常的工作，而且你的当前文档的内容高度必须是高于窗口的高度时，这句代码才能有效的执行。

10、如何禁止用户旋转设备

我曾经也想禁止用户旋转设备，也想实现像某些客户端那样：只能在肖像模式或景观模式下才能正常运行。但现在我可以很负责任的告诉你：别想了!在移动版的webkit中做不到！

至少Apple webapp API已经说到了：我们为了让用户在safari中正常的浏览网页，我们必须保证用户的设备处于任何一个方位时，safari都能够正常的显示网页内容（也就是自适应），所以我们禁止开发者阻止浏览器的orientationchange事件，看来苹果公司的出发点是正确的，苹果确实不是一般的苹果。

iOS已经禁止开发者阻止orientationchange事件，那Android呢？对不起，我没有找到任何资料说Android禁止开发者阻止浏览器orientationchange事件，但是在Android平台，确实也是阻止不了的。

11、如何检测用户是通过主屏启动你的webapp

看过Apple webapp API的同学都知道iOS为safari提供了一个将当前页面添加主屏的功能，按下iphone ipod ipod touch底部工具中的小加号，或者ipad顶部左侧的小加号，就可以将当前的页面添加到设备的主屏，在设备的主屏会自动增加一个当前页面的启动图标，点击该启动图标就可以快速、便捷的启动你的webapp。

从主屏启动的webapp和浏览器访问你的webapp最大的区别是它清除了浏览器上方和下方的工具条，这样你的webapp就更加像是nativeapp了，还有一个区别是window对像中的navigator子对象的一个standalone属性。

iOS中浏览器直接访问站点时，navigator.standalone为false,从主屏启动webapp时，navigator.standalone为true，我们可以通过navigator.standalone这个属性获知用户当前是否是从主屏访问我们的webapp的。 在Android中从来没有添加到主屏这回事！

12、如何关闭iOS中键盘自动大写

我们知道在iOS中，当虚拟键盘弹出时，默认情况下键盘是开启首字母大写的功能的，根据某些业务场景，可能我们需要关闭这个功能，移动版本webkit为input元素提供了autocapitalize，通过指定autocapitalize="off"来关闭键盘默认首字母大写。

13、iOS中如何彻底禁止用户在新窗口打开页面

有时我们可能需要禁止用户在新窗口打开页面，我们可以使用a标签的target="_self"来指定用户在新窗口打开，或者target属性保持空，但是你会发现iOS的用户在这个链接的上方长按3秒钟后，iOS会弹出一个列表按钮，用户通过这些按钮仍然可以在新窗口打开页面，这样的话，开发者指定的target属性就失效了，但是可以通过指定当前元素的-webkit-touch-callout样式属性为none来禁止iOS弹出这些按钮。这个技巧仅适用iOS对于Android平台则无效。

14、iOS中如何禁止用户保存图片＼复制图片

我们在第13条技巧中提到元素的-webkit-touch-callout属性，同样为一个img标签指定-webkit-touch-callout:none，这样用户就无法保存＼复制你的图片了。

15、iOS中如何禁止用户选中文字

我们通过指定文字标签的-webkit-user-select:none便可以禁止iOS用户选中文字。

16、iOS中如何获取滚动条的值

桌面浏览器中想要获取滚动条的值是通过document.scrollTop和document.scrollLeft得到的，但在iOS中你会发现这两个属性是未定义的，为什么呢？因为在iOS中没有滚动条的概念，在Android中通过这两个属性可以正常获取到滚动条的值，那么在iOS中我们该如何获取滚动条的值呢？ 通过window.scrollY和window.scrollX我们可以得到当前窗口的y轴和x轴滚动条的值。

17、如何解决盒子边框溢出

当你指定了一个块级元素时，并且为其定义了边框，设置了其宽度为100％。在移动设备开发过程中我们通常会对文本框定义为宽度100％，将其定义为块级元素以实现全屏自适应的样式，但此时你会发现，该元素的边框(左右)各1个像素会溢了文档，导致出现横向滚动条，为解决这一问题，我们可以为其添加一个特殊的样式-webkit-box-sizing:border-box;用来指定该盒子的大小包括边框的宽度。

18、如何解决Android 2.0以下平台中圆角的问题

如果大家够细心的话，在做wap站点开发时，大家应该会发现android 2.0以下的平台中问题特别的多，比如说边框圆角这个问题吧。

在对一个元素定义圆角时，为完全兼容android 2.0以下的平台，我们必须要按照以下技巧来定义边框圆角：

-webkit这个前缀必须要加上（在iOS中，你可以不加，但android中一定要加）；
如果对针对边框做样式定义，比如border:1px solid #000;那么-webkit-border-radius这属性必须要出现在border属性后。
假如我们有这样的视觉元素，左上角和右上角是圆角时，我们必须要先定义全局的(4个角的圆角值)-webkit-border-radius:5px;然后再依次的覆盖左下角和右下角，-webkit-border-bottom-left-radius:0;-webkit-border-bottom-right-border:0;否则在android 2.0以下的平台中将全部显示直角，还有记住！-webkit这个前缀一定要加上！

19、如何解决android平台中页面无法自适应

虽然你的html和css都是完全自适应的，但有一天如果你发现你的页面在android中显示的并不是自适应的时候，首先请你确认你的head标签中是否包含以下meta标签：

```

<metaname=”viewport”content=”width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=0;”/>
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=0;" />
```

如果有的话，那请你再仔细的看清楚有没有这个属性的值width=device-width，如果没有请立即加上吧！

20、如何解决iOS 4.3版本中safari对页面中5位数字的自动识别和自动添加样式

新的iOS系统也就是4.3版本，升级后对safari造成了一个bug：即使你添加了如下的meta标签，safari仍然会对页面中的5位连续的数字进行自动识别，并且将其重新渲染样式，也就是说你的css对该标签是无效的。

```
<metaname=”format-detection”content=”telphone=no”/>
<meta name="format-detection" content="telphone=no" />
```
我们可以用一个比较龌龊的办法来解决。比如说支付宝wap站点中显示金额的标签，我们都做了如下改写：

```
<buttonclass=”t-balance”style=”background:none;padding:0;border:0;”>95009.00</button>
```
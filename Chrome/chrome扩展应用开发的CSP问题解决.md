chrome 扩展应用开发的时候；项目中为了维护方便，使用了阿里的iconfont；的彩色图标；

如：
```
<script src="https://at.alicdn.com/t/font_k0d09a10gi3blnmi.js"></script>
```
加载外部的资源，chrome会默认拒绝所加载的脚本；原因是，它违反了
```
“script-src ‘self’ blob: filesystem: chrome-extension-resource:”.
```
的内容安全策略；简称CSP（chrome解释：https://developer.chrome.com/extensions/contentSecurityPolicy）

原因分析：

chrome扩展使用了下面这种扩展；

`    "manifest_version":2,`

为了一些安全方面的原因，比如大规模的跨站点脚本攻击等问题，Chrome扩展系统已遵循` Content Security Policy (CSP)`的理念，引入了严格的策略使扩展更安全，同时提供创建和实施策略规则的能力，这些规则被用来控制扩展（或应用）能够加载的资源和执行的脚本。

### 解决思路：

CSP通过黑白名单的机制控制资源加载（或执行脚本）

在Web上，此策略是通过`HTTP`头或`meta`元素定义的。

在Chrome扩展系统中，不存在这两种方式。扩展是通过`manifest.json`文件定义的；

影响范围：

由于CSP策略的影响，以下功能将被限制：

##### 不执行`Inline JavaScript`

   ` Inline JavaScript`和`eval`一样危险，将不会被执行，`CSP`规则将同时禁止内嵌
   
做开发的时候，有用过”juicer”这个jquery插件，用each循环的时候，也报标题的错误，改成字符串拼接，问题可以解决;

##### 加载本地脚本和资源

只有扩展包内的脚本和资源才会被加载！通过Web即时下载的将不会被加载！ 这确保您的扩展只执行已经打包在扩展之中的可信代码，从而避免了线上的网络攻击者通过恶意重定向您所请求的Web资源所带来的安全隐患。

但是通过`manifest`配置，好像也绕不过去；

一怒之下，直接改为相对路径的引用了；完美解决，这也许是`google`的规定把，为了避免风险，全部用包内的文件；

如果有解决的朋友，希望帮忙告知一下；谢谢；
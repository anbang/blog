学习资料：
```
中文翻译：http://zh.discovermeteor.com/chapters/getting-started/ 
官方教程：https://www.meteor.com/tutorials/blaze/creating-an-app 
官网手册：https://guide.meteor.com/ 
Api文档 ：https://docs.meteor.com/ 
社区讨论：https://forums.meteor.com/
```
Meteor不同于传统的MVC，Meteor 可以做更多很酷的事情，其中一件主要的就是 Metoer 变得数据库无处不在。简单说，Meteor 把你的数据拿出一部分子集复制到客户端。

这样后两个主要结果：

- 第一，服务器不再发送 HTML 代码到客户端，而是发送真实的原始数据，让客户端决定如何处理线传数据。 
- 第二，你可以不必等待服务器传回数据，而是立即访问甚至修改数据

************************* 初始化项目 **************************

meteor初始化项目（下面代码直接自带脚手架）

```
    meteor create meteor-demo
```
会在当前目录下，创建一个meteoe-demo的文件夹（类似vue-cli初始化的项目一样）

参数有

```
meteor create –bare  //空的app
meteor create –full    //脚手架的app
```
启动项目

```
cd meteoe-demo
meteor
```

build后就可以访问：http://localhost:3000/ 这个域名了；

当看到：

Welcome to Meteor!

表示已经跑起来了；

image

************************** 添加代码包 ************************

添加 bootstrap 和 underscore

```
meteor add twbs:bootstrap
meteor add underscore
```

image

安装完成后，会自动引入bootstrap 和underscore（Meteor支持热更新，安装完成后，页面自动会渲染）

##### Meteor 中的代码包有点特殊，分为五种：

- Meteor 核心代码本身分成多个核心代码包（core package），每个 Meteor 应用中都包含，你基本上不需要花费精力来维护它们
- 常规 Meteor 代码包称为“isopack”，或同构代码包（isomorphic package，意味着它们既能在客户端也能在服务器端工作）。第一类代码包例如 accounts-ui 或 appcache 由 Meteor 核心团队维护，与 Meteor 捆绑在一起。
- 第三方代码包就是其他用户开发的 isopack 上传到 Meteor 的代码包服务器上。你可以访问 Atmosphere 或 meteor search 命令来浏览这些代码包。
- 本地代码包（local package）是自己开发的代码包，保存在 /packages 文件夹中。
- NPM 代码包（NPM package）是 Node.js 的代码包，虽不能直接用于 Meteor，但可以在上述几种代码包中使用

*************************** 项目结构和加载顺序 ********************************

```
    ├── .meteor
    ├── client            //在 /client 文件夹中的代码只会在客户端运行。
    │   ├── main.css
    │   ├── main.html
    │   └── main.js
    ├── server            //在 /server 文件夹中的代码只会在服务器端运行。
    ├── public            //请将所有的静态文件（字体，图片等）放置在 /public 文件夹中。
    ├── lib
    ├── node_modules    //包
    ├── package.json
    └── .gitignore
    //其它代码则将同时运行于服务器端和客户端上。
```

Meteor 以什么顺序加载文件也很有用

```
在 /lib 文件夹中的文件将被优先载入。
所有以 main.* 命名的文件将在其他文件载入后载入。
其他文件以文件名的字母顺序载入。
```

***************************** 集合 ************************************************

集合从根本上改变应用程序的数据处理方式。从而不必手动检查数据更改（例如，通过一个 AJAX 调用），再根据这些变化去修改 HTML 页面，Meteor 可以随时检测到数据的更改，并将它无缝地应用到用户界面上。

不同工具的区别：

```
1、终端 （系统自带） 
2、浏览器控制台（浏览器自带） 
3、Meteor shell（终端里用 moteor shell 启动） 
4、Mongo shell（终端里用meteor mongo 启动）

```
Mongodb的简单语法（用在Mongo shell里）

- db.posts.insert({}); 插入数据
- db.posts.find();  查找数据库里的数据信息

浏览器控制台操作db；

- Posts.findOne();查看第一条数据
- Posts.find().count(); 查看数据条数
- Posts.find().fetch(); 查看所有数据
- Posts.insert({title: “A second post”}); 插入数据，返回当前的ID

清空数据库：停用meteor服务，输入 meteor reset  ,这样就情况db了，重启meteor服务即可；

演示 server 端代码；

```
//如果数据库是空的，载入3条帖子,如果没有 meteor reset 则不会插入的；
if(Posts.find().count() === 0){
    Posts.insert({
        title: 'server插入的db数据 111111',
        url: 'http://sachagreif.com/introducing-telescope/'
    });
    
    Posts.insert({
        title: 'server插入的db数据 22222',
        url: 'http://meteor.com'
    });
    
    Posts.insert({
        title: 'server插入的db数据 33333',
        url: 'http://themeteorbook.com'
    });
}

```
演示 client 端的代码

```
Template.postList.helpers({
    posts:Posts.find()
});

```
************************* 订阅和发布 *********************

上面一直都是使用autopublish发布的，这个不是为正是环境准备的（autopublish 的目的是让 Meteor 应用有个简单的起步阶段，它简单地直接把服务器上的全部数据镜像到客户端，因此你就不用管发布和订阅了，但是这样带来了安全隐患和更多的内存消耗。）

Meteor里可以理解成数据库无处不在。简单说，Meteor 把你的数据拿出一部分子集复制到客户端。
这样后两个主要结果：

```
第一，服务器不再发送 HTML 代码到客户端，而是发送真实的原始数据，让客户端决定如何处理线传数据。
第二，你可以不必等待服务器传回数据，而是立即访问甚至修改数据（延迟补偿 latency compensation）。
```
名词解释：

发布：就是服务器发布的数据；一个 App 的数据库可能用上万条数据，其中一些还可能是私用和保密敏感数据。无论是安全原因还是扩展性原因，我们不能简单地把数据库镜像到客户端去。

订阅：

需求：卸载 autopublish ，自己写服务端的发布和客户端的订阅；

##### 卸载autopublish ：

```
meteor remove autopublish

```
服务端发布代码 (puhlication.js)

```
Meteor.publish('posts',function () {
    return Posts.find();
});

```
客户端订阅代码（main.js）

```
Meteor.subscribe('posts');
```

我们可以把发布/订阅模式想象成为一个漏斗，从服务器端（数据源）过滤数据传送到客户端（目标）。

这个漏斗的专属协议叫做 DDP（分布式数据协议 Distributed Data Protocol 的缩写）

下面是 发布和订阅某个作者 的 / 使用函数传参：

```
// 在服务器端
Meteor.publish('posts', function(author) {
    return Posts.find({flagged: false, author: author});
});
// 在客户端
Meteor.subscribe('posts', 'bob-smith'); 
```
另外一种

```
//同时使用两种技术，只发布作者是 Tom 的帖子，并且隐藏 date 日期字段:
Meteor.publish('allPosts', function(){
    return Posts.find({'author': 'Tom'}, {fields: {
        date: false
    }});
}); 
```
**************************** 路由 （iron-router ） ***********************

Meteor是使用的是 iron-router 这个路由包；

DOC：https://iron-meteor.github.io/iron-router/

名次解释：

- 路由规则（Route）：路由规则是路由的基本元素。它的工作就是当用户访问 App 的某个 URL 的时候，告诉 App 应该做什么，返回什么东西。
- 路径（Path）：路径是访问 App 的 URL。它可以是静态的（/terms_of_service）或者动态的（/posts/xyz），甚至还可以包含查询参数（/search?keyword=meteor）。
- 目录（Segment）：路径的一部分，使用正斜杠（/）进行分隔。
- Hooks：Hooks 是可以执行在路由之前，之后，甚至是路由正在进行的时候。一个典型的例子是，在显示一个页面之前检测用户是否拥有这个权限。
- 过滤器（Filter）：过滤器类似于 Hooks ，为一个或者多个路由规则定义的全局过滤器。
- 路由模板（Route Template）：每个路由规则指向的 Meteor 模板。如果你不指定，路由器将会默认去寻找一个具有相同名称的模板。
- 布局（Layout）：你可以想象成一个数码相框的布局。它们包含所有的 HTML 代码放置在当前的模板中，即使模板发生改变它们也不会变。
- 控制器（Controller）：有时候，你会发现很多你的模板都在重复使用一些参数。为了不重复你的代码，你可以让这些路由规则继承一个路由控制器（Routing Controller）去包含所有的路由逻辑。

安装路由

```
meteor add iron:router
```

清空main.html（main作为公用的头部）

```
<head>
    <title>这是一个测试Title</title>
</head> 
```
创建layout 的 template （client/templates/application/layout.html）

```
<template name="layout">
    <div class="container">
        <header class="navbar navbar-default" role="navigation">
            <div class="navbar-header">
                <a href="/" class="navbar-brand">测试的标题啊</a>
            </div>
        </header>
        <div id="main" class="row-fluid">
            {{> yield}}
        </div>
    </div>
</template> 
```
创建router（lib/router.js）

```
//第一，我们告诉路由器使用我们刚刚创建的 layout 模板作为所有路由的默认布局。
Router.configure({
    layoutTemplate:"layout"
});
    
//定义了一个名为 postList 的路由规则，并映射到 / 路径
Router.route("/",{name:"postList"});
```

放在 /lib 文件夹里面的所有文件都会在你的 App 运行的时候确保首先被加载；不过有一点注意的是：因为 /lib 文件夹并不是放在 /client 或 /server 文件夹里面，这意味着它的代码将会同时存在于客户端和服务器。

问题：现在列表都是空一段时间然后再加载（因为要等到 posts 订阅完成后，即从服务器抓取完帖子的数据，才能有帖子显示在页面上。）

我们可以改为：把订阅放到 waitOn 的返回上（以前是写在main.js中的）。

修改路由：（lib/router.js）

```
    //第一，我们告诉路由器使用我们刚刚创建的 layout 模板作为所有路由的默认布局。
    Router.configure({
        layoutTemplate:"layout",
        waitOn:function () {
            return Meteor.subscribe('allPosts');
        }
    });
     
    //定义了一个名为 postList 的路由规则，并映射到 / 路径
    Router.route("/",{name:"postList"});
```

注意，因为我们在路由器级别上全局定义了 waitOn 方法，所以这个只会在用户第一次访问你的 App 的时候发生一次。在那之后，数据已经被加载到了浏览器的内存，路由器不需要再次去等待它。

使用 spin 包去创建loading效果 ；

    添加：meteor add sacha:spin

然后在 client/templates/includes 文件夹内创建 loading 模板：

```
    <template name="loading">
        {{>spinner}}
    </template>
```

router中配置 loading 模板；

```
    //第一，我们告诉路由器使用我们刚刚创建的 layout 模板作为所有路由的默认布局。
    Router.configure({
        layoutTemplate:"layout",
        loadingTemplate:"loading",
        waitOn:function () {
            return Meteor.subscribe('allPosts');
        }
    });
     
    //定义了一个名为 postList 的路由规则，并映射到 / 路径
    Router.route("/",{name:"postList"});
```

这样就加载好了；注意 {{> spinner}} 是 spin 包中的一个模板标签。尽管这部分是来自我们的 App 之外，不过我们就像其他模板一样去使用它就可以了。

##### 动态路由的添加；

文章的页面，就是动态路由实现的；

```
router文件如下：

    //定义了一个名为 postList 的路由规则，并映射到 / 路径
    Router.route("/",{name:"postList"});
    Router.route("/posts/:_id",
        {
            name:"postPage",
            data:function () {
                //从 URL 上获取 _id ，并通过它获得正确的数据源（否则就没有数据用了）：
                return Posts.findOne(this.params._id);
            }
        }
    ); 
```
post_page 文件如下：

```
    <template name="postPage">
      <div class="post-page page">
        {{> postItem}}
      </div>
    </template>
```

当然这需要在列表里的配置链接地址，post_list的入口如下

```
    <a href="{{pathFor 'postPage'}}" class="discuss btn btn-default">Discuss</a>
```

404页面的添加：

404页面分为2种，

一种是错误的rul；比如：http://localhost:3000/postsssssssss/

一种是正确的url，但是找不到数据，比如文章页路径：http://localhost:3000/posts/11111111；

首先在application添加个模板；

```
<template name="notFound">
    <div class="not-found jumbotron">
        <h2>404</h2>
        <p>抱歉，我们无法找到该页面。</p>
    </div>
</template>
```

然后修改router如下；

```
Router.configure({
    layoutTemplate:"layout",
    loadingTemplate:"loading",
    notFoundTemplate:'notFound',
    waitOn:function () {
        return Meteor.subscribe('allPosts');
    }
});
Router.onBeforeAction('dataNotFound',{only:"postPage"});//没有找到数据的时候
```

******** 消息会话 Meteor session *******************

首先是需要安装session这个包的；

```
meteor add session
```

这样安装好以后就可以正常使用了；否则会报错；Session is not defined；

注意：Session 对象不在用户之间共享，甚至在浏览器标签之间 。

Session的设置和获取如下:

```
Session.set('pageTitle', 'A brand new title'); 
Session.get('pageTitle');
```

从中我们应该要学会：

- 应该在 Session 或者 URL 中存储用户状态。从而在动态代码重载的时候，让用户发生中断的机会降到最低。
- 尽可能使用 URL 去存储你想要共享在用户之间的状态。

**************** 添加用户 *********************

首先要终端安装包文件

```
meteor add ian:accounts-ui-bootstrap-3
meteor add accounts-password
```

通过 `{{loginButtons}}`helper 在我们的网站中使用，把header从layout中分隔开

```
<template name="header">
    <nav class="navbar navbar-default" role="navigation">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navigation">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="{{pathFor 'postsList'}}">Microscope</a>
        </div>
        <div class="collapse navbar-collapse" id="navigation">
            <ul class="nav navbar-nav navbar-right">
                {{> loginButtons}}
            </ul>
        </div>
    </nav>
</template>
```

如何告诉我们的账户系统要求用户需要通过用户名密码来登录？只需配置一个 Accounts.ui 模块到一个名叫 config.js 的文件里面，并把文件放在 `client/helpers/` 中：

```
Accounts.ui.config({
    passwordSignupFields: 'USERNAME_ONLY'
});
```

常见的查命令（ 浏览器控制台）

```
Meteor.users.findOne();
Meteor.users.find().count();
```

mongo命令

```
db.users.count() 
db.users.find()
```

**************** 响应式 ***************

Meteor 是一个实时性、响应式的框架，但并不是所有的代码在 Meteor App 里面都是响应式的。如果是这样，当有任何数据发生改变时，你的 App 都会自动进行重新运行。相反，响应式只是在特定区域的代码中发生，我们称这些区域为 Computations 。
~


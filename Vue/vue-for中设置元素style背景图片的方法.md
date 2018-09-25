需求：需要动态设置某个DOM的背景图片；

使用的VUE2，在v-for的时候，一般都是

`<h3>{{item.title}}</h3>`

或者直接 : src=

```
:src = item.target.cover_url 
```

这样的写法；

但是遇到一个特殊场景，渲染出background-image 这样的，就不能再用上面的方法了；

直接上代码

``` 
<router-link to="/?" class="feed-item" tag="a" v-for="(item,index) in recommend_feeds">
  <!-- 布局1 -->
  <div v-if="item.layout !== 5">
    <div class="feed-content">
      <div class="cover"
           style="position: relative; background: center center / cover no-repeat rgb(250, 250, 250);"
           :style="{backgroundImage: 'url(' + item.target.cover_url + ')'}"
      >
        <div style="padding-top: 100%"></div>
      </div>
      <h3>{{item.title}}</h3>
      <p>{{item.target.desc}}</p>
    </div>
    <div class="author">
      <span class="name">by {{item.target.author.name}}</span>
    </div>
    <span class="feed-label">{{item.source_cn}}</span>
  </div>
 
  <!-- 布局5 -->
  <div v-else="item.layout === 5">
    <div class="feed-content">
      <div class="photos">
        <div class="main"
             style="position: relative; background: center center / cover no-repeat rgb(250, 250, 250);"
             :style="{backgroundImage: 'url(' + item.target.cover_url + ')'}"
        >
          <div></div>
        </div>
        <div class="aside">
          <div class="aside-pic">
            <div
              style="position: relative; background: center center / cover no-repeat rgb(250, 250, 250);"
              :style="{backgroundImage: 'url(' + item.target.more_pic_urls[0] + ')'}"
            >
              <div style="padding-top: 100%;"></div>
            </div>
          </div>
          <div class="aside-pic">
            <div
              style="position: relative; background: center center / cover no-repeat rgb(250, 250, 250);"
              :style="{backgroundImage: 'url(' + item.target.more_pic_urls[1] + ')'}"
            >
              <div style="padding-top: 100%;"></div>
            </div>
            <div class="more-pic">
              <span class="count">
                {{item.target.photos_count-3}}+
              </span>
            </div>
          </div>
        </div>
      </div>
      <h3>{{item.title}}</h3>
    </div>
    <div class="author">
      <span class="name">by {{item.target.author.name}}</span>
    </div>
    <span class="feed-label">{{item.source_cn}}</span>
  </div>
</router-link>
```

核心代码是下面的这一段

``` 
<div class="cover"
     style="position: relative; background: center center / cover no-repeat rgb(250, 250, 250);"
     :style="{backgroundImage: 'url(' + item.target.cover_url + ')'}"
>
  <div style="padding-top: 100%"></div>
</div>
```

这里是先设置了普通的style；

因为图片的地址是需要解析的；所以不能写在默认的style里；我们需要在 :style 里面（这里面的会动态加载并解析进去）

需要用到的是backgroundImage 这个驼峰写法，不要写错了（默认的属性 background-image  是不行的）



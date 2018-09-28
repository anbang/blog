做了类似引导分享的一个箭头；

提醒用户去分享；

如下：
```css
.share-prompt-wrap .share-prompt-arrow{
    width: 0.573rem;
    -webkit-animation-duration:3s;
    animation-duration:3s;
    -webkit-animation-iteration-count:infinite;
    animation-iteration-count:infinite;
    -webkit-animation-name:share-prompt-animation;
    animation-name:share-prompt-animation;
    -webkit-animation-timing-function:ease-in-out;
    animation-timing-function:ease-in-out;
}
```

动画部分如下

```css
@-webkit-keyframes share-prompt-animation {
    0% {
        -webkit-transform: none;
        transform: none;
    }
    50% {
        -webkit-transform: none;
        transform: none;
    }
    61% {
        -webkit-transform: translate3d(0, 0, 0) ;
        transform: translate3d(0, 0, 0) ;
    }
    70% {
        -webkit-transform:  translate3d(150%, -150%, 0) ;
        transform: translate3d(150%, -150%, 0) ;
    }
    71% {
        -webkit-transform: translate3d(300%, 0, 0);
        transform: translate3d(300%, 0, 0);
    }
    72% {
        -webkit-transform: translate3d(0, 300%, 0);
        transform: translate3d(0, 300%, 0);
    }
    73% {
        -webkit-transform: translate3d(-120%, 120%, 0);
        transform: translate3d(-120%, 120%, 0) ;
    }
    100% {
        -webkit-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0) ;
    }
}
@keyframes share-prompt-animation {
    0% {
        -webkit-transform: none;
        transform: none;
    }
    50% {
        -webkit-transform: none;
        transform: none;
    }
    61% {
        -webkit-transform: translate3d(0, 0, 0) ;
        transform: translate3d(0, 0, 0) ;
    }
    70% {
        -webkit-transform:  translate3d(150%, -150%, 0) ;
        transform: translate3d(150%, -150%, 0) ;
    }
    71% {
        -webkit-transform: translate3d(300%, 0, 0);
        transform: translate3d(300%, 0, 0);
    }
    72% {
        -webkit-transform: translate3d(0, 300%, 0);
        transform: translate3d(0, 300%, 0);
    }
    73% {
        -webkit-transform: translate3d(-120%, 120%, 0);
        transform: translate3d(-120%, 120%, 0) ;
    }
    100% {
        -webkit-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0) ;
    }
    
}
```

这样就可以了；

*************************扩展*********************

顺便记录下，摇摆的CSS写法；类似手机摇一摇的效果

```css
.shake-welfare {
    -webkit-animation-duration:1.5s;
    animation-duration:1.5s;
    -webkit-animation-iteration-count:infinite;
    animation-iteration-count:infinite;
    -webkit-animation-name:get-welfare-animation;
    animation-name:get-welfare-animation;
    -webkit-animation-timing-function:ease-in-out;
    animation-timing-function:ease-in-out;
    /*-webkit-transition-delay:2s;
    transition-delay:2s;*/
}
```

然后就是CSS的写法

```css
@-webkit-keyframes get-welfare-animation {
    0% {
        -webkit-transform: none;
        transform: none;
    }
    66% {
        -webkit-transform: none;
        transform: none;
    }
    
    69% {
        -webkit-transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
        transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
    }
    
    72% {
        -webkit-transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
        transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
    }
    
    75% {
        -webkit-transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
        transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
    }
    
    78% {
        -webkit-transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
        transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
    }
    
    81% {
        -webkit-transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
        transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
    }
    84% {
        -webkit-transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
        transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
    }
    
    87% {
        -webkit-transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
        transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
    }
    
    90% {
        -webkit-transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
        transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
    }
    
    93% {
        -webkit-transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
        transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
    }
    
    95% {
        -webkit-transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
        transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
    }
    
    100% {
        -webkit-transform: none;
        transform: none;
    }
}
@keyframes get-welfare-animation {
    0% {
        -webkit-transform: none;
        transform: none;
    }
    66% {
        -webkit-transform: none;
        transform: none;
    }
    69% {
        -webkit-transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
        transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
    }
    
    72% {
        -webkit-transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
        transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
    }
    
    75% {
        -webkit-transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
        transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
    }
    
    78% {
        -webkit-transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
        transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
    }
    
    81% {
        -webkit-transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
        transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
    }
    84% {
        -webkit-transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
        transform: translate3d(-25%, 0, 0) rotate3d(0, 0, 1, -5deg);
    }
    
    87% {
        -webkit-transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
        transform: translate3d(20%, 0, 0) rotate3d(0, 0, 1, 3deg);
    }
    
    90% {
        -webkit-transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
        transform: translate3d(-15%, 0, 0) rotate3d(0, 0, 1, -3deg);
    }
    
    93% {
        -webkit-transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
        transform: translate3d(10%, 0, 0) rotate3d(0, 0, 1, 2deg);
    }
    
    95% {
        -webkit-transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
        transform: translate3d(-5%, 0, 0) rotate3d(0, 0, 1, -1deg);
    }
    
    100% {
        -webkit-transform: none;
        transform: none;
    }
}

```
~~~~
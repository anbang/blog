git clone 的时候遇到的报错；

``` 
Connection to bitbucket.org closed by remote host.KiB/s
```

搜所了下，

尝试解决办法：git config –global http.postBuffer 1048576000

参考资源；

http://www.cnblogs.com/Aimeast/p/3515560.html

http://www.jianshu.com/p/ae1eb021fd20

http://blog.csdn.net/tallercc/article/details/53870171

https://stackoverflow.com/questions/6842687/the-remote-end-hung-up-unexpectedly-while-git-cloning
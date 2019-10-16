**根据访问路径 访问不同的项目**

```
location /test1 {
    proxy_pass http://ip:8080/project1;
}

location /test2 {
    proxy_pass http://ip:8888/project2;
}
```
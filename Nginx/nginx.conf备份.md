#### 刚配置gzip压缩的

路径: `/etc/nginx/nginx.conf` 内容如下：

```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;
    gzip  on;   #开启gzip
    gzip_min_length 1k; #低于1kb的资源不压缩
    gzip_comp_level 8; #压缩级别【1-9】，越大压缩率越高，同时消耗cpu资源也越多，建议设置在4左右。
    gzip_types text/plain application/javascript application/x-javascript text/javascript text/xml text/css;  #需要压缩哪些响应类型的资源，多个空格隔开。不建议压缩图片，下面会讲为什么。
    gzip_disable "MSIE [1-6]\.";  #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
    gzip_vary on;  #是否添加“Vary: Accept-Encoding”响应头

    include /etc/nginx/conf.d/*.conf;
}
```

文件 `/etc/nginx/conf.d/default.conf` 如下

```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```


### www重定向

www 重定向到转向不带 www：

```
server {
    server_name www.axihe.com;
    return 301 $scheme://axihe.com$request_uri;
}
```

不带 www 重定向到带 www：

```
server {
    server_name axihe.com;
    return 301 $scheme://www.axihe.com$request_uri;
}
```

`/etc/nginx/conf.d/default.conf`文件如下

```
server {
    listen       80;
    server_name axihe.com;
    return 301 $scheme://www.axihe.com$request_uri;
    
    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/axihe-web/dist;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```


`/etc/nginx/conf.d/www-axihe-com`文件如下

```
server {
  listen       80;
  server_name www.axihe.com;
  root /usr/share/nginx/axihe-web/dist;
  index index.html;
  location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|js|pdf|txt){
    root /usr/share/nginx/axihe-web/dist;
  }
}

```

### ssl setting

```
server {
  listen       80;
  listen       443 ssl;      # 对 443 端口进行 SSL 加密
  server_name  www.axihe.com;
  root /usr/share/nginx/axihe-web/dist;
  index index.html;
  location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|js|pdf|txt){
    root /usr/share/nginx/axihe-web/dist;
  }

  # 沃通生成的 SSL 证书的存放位置
  ssl_certificate           /etc/nginx/cert/1874900_www.axihe.com.pem;
  ssl_certificate_key       /etc/nginx/cert/1874900_www.axihe.com.key;
  # 其他 SSL 相关设置
  ssl_session_timeout       10m;
  ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers               ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_prefer_server_ciphers on;

  # 主域名和子域名都启用 HSTS，过期时间为两年
  add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";

  # 所有 HTTP 访问都永久重定向（301）到 HTTPS
  if ( $scheme = http ) {
    rewrite ^/(.*) https://$server_name/$1 permanent;
  }
}
```

## 增加谷歌分析
`/etc/nginx/conf.d/www-axihe-com`文件如下
```
server {
  listen       80;
  listen       443 ssl;      # 对 443 端口进行 SSL 加密
  server_name  www.axihe.com;
  # root /usr/share/nginx/axihe-web/dist;
  # index index.html;

  # google any start
  userid on;
  userid_name cid;
  userid_domain axihe.com;
  userid_path /;
  userid_expires max;

  location @ga {
    internal;
    resolver 8.8.8.8 [2001:4860:4860::8888];

    set $lang 'zh-cn';
    if ( $http_accept_language ~ en ) {
        set $lang 'en-us';
    }

    proxy_method GET;
    # UA-100341141-2
    proxy_pass https://ssl.google-analytics.com/collect?v=1&tid=UA-100341141-2&$uid_set$uid_got&t=pageview&dh=$host&dp=$uri&uip=$remote_addr&dr=$http_referer&z=$msec&ul=$lang;
    proxy_set_header User-Agent $http_user_agent;
    proxy_pass_request_headers off;
    proxy_pass_request_body off;
  }
  # google any end

  location / {
    root /usr/share/nginx/axihe-web/dist;
    index index.html;
    # 当匹配到此 location 时，这里会异步的调用 @ga
    if ($http_user_agent !~* "qihoobot|Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Mediapartners-Google|Adsbot-Google|Feedfetcher-Google|Yahoo! Slurp|Yahoo! Slurp China|YoudaoBot|Sosospider|Sogou spider|Sogou web spider|MSNBot|ia_archiver|Tomato Bot|YiSou Spider") {
        post_action @ga;
    }
    # post_action @ga;
  }

  # 沃通生成的 SSL 证书的存放位置
  ssl_certificate           /etc/nginx/cert/1874900_www.axihe.com.pem;
  ssl_certificate_key       /etc/nginx/cert/1874900_www.axihe.com.key;
  # 其他 SSL 相关设置
  ssl_session_timeout       10m;
  ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers               ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_prefer_server_ciphers on;

  # 主域名和子域名都启用 HSTS，过期时间为两年
  add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";

  # 所有 HTTP 访问都永久重定向（301）到 HTTPS
  if ( $scheme = http ) {
    rewrite ^/(.*) https://$server_name/$1 permanent;
  }
}
```

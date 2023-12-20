
### 1、nginx\conf\nginx.conf

```bash

worker_processes  1;

events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    # 隐藏NGINX版本号
    server_tokens off;

    server {
        listen 443 ssl;
        #配置HTTPS的默认访问端口为443。
        #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
        #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
        server_name  serverName;
        root html;
        index index.html index.htm;
        # SSL相关配置
        ssl_certificate sslCertificate;  
        ssl_certificate_key sslCertificateKey; 
        ssl_session_timeout 5m;
        ssl_ciphers ssl_ciphers;
        #表示使用的加密套件的类型。
        ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3; #表示使用的TLS协议的类型，您需要自行评估是否配置TLSv1.1协议。
        ssl_prefer_server_ciphers on;

        server_tokens off;

        location / {
            root         "D:\study";
            index        index.html;
        }              

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}

```

![nginx基本配置_1](../../images/nginx/nginx基本配置.png)

### 2、配置SSL

将证书放到`nginx\conf\cert`文件夹下。

### 3、根据标识跳转到不同应用程序

修改`nginx.conf`文件

```bash
http {
  server {
    location /application {
      set $is_mobile false;   #设置一个初始值
 
      if ( $http_cookie ~* "ACCESS_TERMINAL=mobile" ) {    #判断匹配手机端
        set $is_mobile true;
      }
      if ($http_user_agent ~* (android|ip(ad|hone|od)|kindle|blackberry|windows\s(ce|phone))) {    #匹配手机端类型
        set $is_mobile true;
      }
      if ($is_mobile = true) {
        root   "D:\application\app";
        break;
      }
      root "D:\application\web";
    }
  }
}

```


### 4、反向代理到其他域名

注意：api前后需要有/，proxy_pass域名后面也有/。

```bash
http {
  server {
    location /api/ {
      proxy_pass https://algolia.net/;
    }
  }
}
```

### 5、配置开启gzip

```bash
http {
  gzip  on;
  gzip_buffers 16 8k;
  gzip_comp_level 6;
  gzip_http_version 1.1;
  gzip_min_length 256;
  gzip_proxied any;
  gzip_vary on;
  gzip_types
  text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml     text/javascript application/javascript application/x-javascript     text/x-json application/json application/x-web-app-manifest+json     text/css text/plain text/x-component     font/opentype application/x-font-ttf application/vnd.ms-fontobject     image/x-icon;
  gzip_disable  "msie6";

  server { 
    gzip on;
    gzip_buffers 32 4K;
    gzip_comp_level 6;
    gzip_min_length 100;
    gzip_types application/javascript text/css text/xml;
    gzip_disable "MSIE [1-6]\."; #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
    gzip_vary on;

    location / {
      gzip_http_version 1.0;
      proxy_set_header Accept-Encoding gzip;
    }
  }
}
```



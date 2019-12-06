Modify Linux Server [docker-freshrss](https://github.com/linuxserver/docker-freshrss) Dockerfile.

Based on [iobase-php](https://github.com/lihsai0/docker-iobase-php)(Modified from Linux Server [iobase/nginx:3.10](https://github.com/linuxserver/docker-baseimage-alpine-nginx))

## Usage

``` bash
docker run -d \
  --name=freshrss \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 9000:9000 \
  -v /srv/fresh-rss:/config \
  --restart unless-stopped \
  fresh-rss
```

## Nginx config file example

``` conf
server {
  server_name fresh-rss.example.com;
  listen 80;
  # listen 443 ssl http2;
  
  root /srv/fresh-rss/www/freshrss/p;
  index index.php index.html index.htm;

  # ssl_certificate /path/to/fresh-rss.example.com.pem;
  # ssl_certificate_key /path/to/fresh-rss.example.com.key;
  # ssl_session_timeout 10m;
  # ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  # ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  # ssl_prefer_server_ciphers on;

  access_log /path/to/fresh-rss.example.com_nginx.log combined;

  # if ($ssl_protocol = "") { return 301 https://$host$request_uri; }

  # location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
  #   expires 30d;
  #   access_log off;
  # }
  # location ~ .*\.(js|css)?$ {
  #   expires 7d;
  #   access_log off;
  # }

  client_max_body_size 0;

  location ~ ^.+?\.php(/.*)?$ {
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    include /usr/local/nginx/conf/fastcgi_params;
    fastcgi_param SCRIPT_FILENAME /config/www/freshrss/p/$fastcgi_script_name;
  }
  location / {
    try_files $uri $uri/ index.php;
  }
}

```

## Other

[$TZ Values](https://www.php.net/manual/en/timezones.php)

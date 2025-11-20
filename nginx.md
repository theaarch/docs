# Nginx

```bash
sudo nano /etc/nginx/conf.d/example.com.conf
```

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /var/www/example.com/public;

    #add_header X-Frame-Options "SAMEORIGIN";
    #add_header X-Content-Type-Options "nosniff";

    index index.php index.html index.htm;

    #include rewrite/laravel.conf;

    #location = /favicon.ico { access_log off; log_not_found off; }
    #location = /robots.txt  { access_log off; log_not_found off; }
 
    #error_page 404 /index.php;

    include php.conf;
    
    location ~ /\.(?!well-known).* {
        deny all;
    }

    access_log /var/log/nginx/example.com.log;
    error_log /var/log/nginx/example.com.error.log;

    #include alb.conf;
}
```

```bash
sudo nano /etc/nginx/rewrite/laravel.conf
```

```nginx
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
```

```bash
sudo nano /etc/nginx/php.conf
```

```nginx
    location ~ [^/]\.php(/|$) {
        try_files $uri =404;
        fastcgi_index index.php;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
```

```bash
sudo nano /etc/nginx/alb.conf
```

```nginx
    set_real_ip_from 172.31.0.0/16;
    real_ip_header X-Forwarded-For;
    real_ip_recursive on;
```

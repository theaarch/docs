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

    #listen 443 ssl;
    #listen [::]:443 ssl;
    #ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    #ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    #include /etc/letsencrypt/options-ssl-nginx.conf;
    #ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
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
sudo nano /etc/nginx/php8.4.conf
```

```nginx
    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
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

```bash
sudo nano /etc/nginx/rewrite/spa.conf
```

```nginx
    location / {
        try_files $uri $uri/ /index.html?$query_string;
    }
```



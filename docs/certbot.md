# Certbot

```bash
sudo apt install certbot python3-certbot-nginx
```

```bash
sudo certbot certonly --nginx -d example.com -d www.example.com
```

```nginx
server {
    # ...

    listen [::]:443 ssl;
    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    # ...
}
```

```nginx
server {
    if ($host = example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    listen 80 ;
    listen [::]:80 ;
    server_name example.com;
    return 404; # managed by Certbot
}
```

```bash
sudo systemctl status certbot.timer
```

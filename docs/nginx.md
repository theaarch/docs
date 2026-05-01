# Nginx

```bash
sudo apt install nginx
```

```bash
sudo nano /etc/nginx/sites-available/example.com
```

```bash
# Link to sites-enabled
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/

# Test configuration for syntax errors
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx
```


```bash
getent passwd www-data
```

```
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

# Nginx

```bash
sudo apt install nginx
```

```bash
sudo nano /etc/nginx/sites-available/example.com
```

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

```bash
sudo nginx -t
```

```bash
sudo systemctl reload nginx
```

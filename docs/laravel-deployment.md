# Laravel Deployment

Deploying a Laravel application to production requires a few manual steps to ensure your server is configured correctly. This guide provides a comprehensive overview of the necessary configurations for Nginx, permissions, database setup, and background workers.

## Server Requirements

Before you begin, ensure your server meets the following requirements:

- **PHP** >= 8.2
- **Composer** (for managing dependencies)
- **Nginx** (as the web server)
- **MySQL/PostgreSQL** (for the database)
- **Supervisor** (optional, recommended for queue workers)

## Nginx Configuration

Nginx is the most popular web server for hosting Laravel applications. Below is a standard configuration file.

Ensure you replace `example.com` with your actual domain and `/srv/example.com/public` with the path to your application's `public` directory.

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /srv/example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

## Directory Permissions

Laravel requires write permissions for certain directories to function correctly. Specifically, the `storage` and `bootstrap/cache` directories must be writable by the web server user (usually `www-data`).

```bash
# Set ownership to the web server user
sudo chown -R www-data:www-data /srv/example.com

# Ensure storage and cache are writable
sudo chmod -R 775 /srv/example.com/storage
sudo chmod -R 775 /srv/example.com/bootstrap/cache
```

## Database Configuration

You will need to create a database and a user for your application. Below are example SQL commands for MySQL/MariaDB.

```sql
# Create the database
CREATE DATABASE example_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# Create the user and set a password
CREATE USER 'example_user'@'%' IDENTIFIED BY 'your-secure-password';

# Grant privileges
GRANT ALL PRIVILEGES ON example_db.* TO 'example_user'@'%';

# Apply changes
FLUSH PRIVILEGES;
```

Update your `.env` file with these credentials:

```dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=example_db
DB_USERNAME=example_user
DB_PASSWORD=your-secure-password
```

## Optimization

In production, you should cache your configuration, events, routes, and views for better performance.

```bash
php artisan config:cache
php artisan event:cache
php artisan route:cache
php artisan view:cache
```

> [!WARNING]
> If you are using `config:cache`, ensure you are only calling `env()` from within your configuration files, not from your application code.

## Task Scheduling

Laravel's task scheduler requires a single Cron entry to run every minute.

```bash
sudo -u www-data crontab -e
```

Add the following line to the crontab:

```cron
* * * * * cd /srv/example.com && php artisan schedule:run >> /dev/null 2>&1
```

## Queue Workers

If your application uses queues, you should use **Supervisor** to keep your queue workers running.

### 1. Install Supervisor

```bash
sudo apt install supervisor
```

### 2. Configure Supervisor

Create a file at `/etc/supervisor/conf.d/laravel-worker.conf`:

```ini
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /srv/example.com/artisan queue:work sqs --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=www-data
numprocs=8
redirect_stderr=true
stdout_logfile=/srv/example.com/storage/logs/worker.log
stopwaitsecs=3600
```

### 3. Start Supervisor

```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
```

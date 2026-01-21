# Composer



```bash
composer config --global --auth github-oauth.github.com ghp_abcd
```

```bash
composer install --no-dev --prefer-dist --optimize-autoloader
```

```bash
php8.2 /usr/local/bin/composer install --no-dev --prefer-dist --optimize-autoloader
```

```bash
composer install --no-dev --prefer-dist --optimize-autoloader --ignore-platform-reqs
```

## satis

```bash
php ./vendor/bin/satis add --repository-url=git@github.com:user/repo.git
```

```bash
php ./vendor/bin/satis build --repository-url=git@github.com:user/repo.git
```

```bash
composer require user/repo:dev-main
```

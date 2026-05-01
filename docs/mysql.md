# MySQL

```bash
sudo apt install mysql-server
```

```bash
sudo mysql
```

```bash
sudo cp /etc/mysql/mysql.conf.d/mysqld.cnf /etc/mysql/mysql.conf.d/mysqld.cnf.default
```

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

```
bind-address            = 0.0.0.0
```

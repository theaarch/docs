# PostgreSQL

```bash
sudo apt install postgresql
```

```bash
sudo -u postgres psql
```

```bash
sudo cp /etc/postgresql/18/main/postgresql.conf /etc/postgresql/18/main/postgresql.conf.default
```

```bash
sudo nano /etc/postgresql/18/main/postgresql.conf
```

```
listen_addresses = '*'
```

```bash
sudo systemctl restart postgresql
```


```sql
mysqldump -u root -p myapp > myapp.sql
mysql -u root -p myapp < myapp.sql

pg_dump -U postgres myapp > myapp.sql
psql -U postgres -d myapp < myapp.sql

use database_name;
\c database_name

show tables;
\dt
```

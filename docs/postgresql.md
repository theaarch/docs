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

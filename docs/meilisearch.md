# meilisearch

```bash
curl -L https://install.meilisearch.com | sh
```

```bash
mv ./meilisearch /usr/local/bin/
```

```bash
useradd -d /var/lib/meilisearch -s /bin/false -m -r meilisearch
```

```bash
chown meilisearch:meilisearch /usr/local/bin/meilisearch
```

```bash
mkdir /var/lib/meilisearch/data /var/lib/meilisearch/dumps /var/lib/meilisearch/snapshots
chown -R meilisearch:meilisearch /var/lib/meilisearch
chmod 750 /var/lib/meilisearch
```

```bash
curl https://raw.githubusercontent.com/meilisearch/meilisearch/latest/config.toml > /etc/meilisearch.toml
```

```
env = "production"
master_key = "MASTER_KEY"
db_path = "/var/lib/meilisearch/data"
dump_dir = "/var/lib/meilisearch/dumps"
snapshot_dir = "/var/lib/meilisearch/snapshots"
```

```bash
cat << EOF > /etc/systemd/system/meilisearch.service
[Unit]
Description=Meilisearch
After=systemd-user-sessions.service

[Service]
Type=simple
WorkingDirectory=/var/lib/meilisearch
ExecStart=/usr/local/bin/meilisearch --config-file-path /etc/meilisearch.toml
User=meilisearch
Group=meilisearch
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF
```

```bash
systemctl enable meilisearch
systemctl start meilisearch
```

```bash
systemctl status meilisearch
```


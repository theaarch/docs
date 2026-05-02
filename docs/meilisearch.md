# Meilisearch

## Step 1: Install Meilisearch

Log into your EC2 instance via SSH and update the system packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl -y
```

Next, use `curl` to download and run the Meilisearch command-line installer:

```bash
curl -L https://install.meilisearch.com | sh
```

Move the binary file into `/usr/local/bin` to make it accessible from anywhere:

```bash
sudo mv ./meilisearch /usr/local/bin/
```

## Step 2: Create system user

Running applications as root exposes you to unnecessary security risks. Create a dedicated user for Meilisearch:

```bash
sudo useradd -d /var/lib/meilisearch -s /bin/false -m -r meilisearch
```

Give the new user ownership of the Meilisearch binary:

```bash
sudo chown meilisearch:meilisearch /usr/local/bin/meilisearch
```

## Step 3: Create a configuration file

Create the directories where Meilisearch will store its data:

```bash
sudo mkdir -p /var/lib/meilisearch/data /var/lib/meilisearch/dumps /var/lib/meilisearch/snapshots
sudo chown -R meilisearch:meilisearch /var/lib/meilisearch
sudo chmod 750 /var/lib/meilisearch
```

Download the default configuration file:

```bash
curl https://raw.githubusercontent.com/meilisearch/meilisearch/latest/config.toml | sudo tee /etc/meilisearch.toml > /dev/null
```

Edit `/etc/meilisearch.toml` and update the following lines, replacing `MASTER_KEY` with a secure 16-byte string:

```
env = "production"
master_key = "MASTER_KEY"
db_path = "/var/lib/meilisearch/data"
dump_dir = "/var/lib/meilisearch/dumps"
snapshot_dir = "/var/lib/meilisearch/snapshots"
```

## Step 4: Run Meilisearch as a service

Create a systemd service file to run Meilisearch as a background service:

```bash
sudo cat << EOF > /etc/systemd/system/meilisearch.service
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

Reload systemd, then enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable meilisearch
sudo systemctl start meilisearch
```

Verify the service is running:

```bash
sudo systemctl status meilisearch
```

You should see a message confirming your service is active and running.

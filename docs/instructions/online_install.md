# SCOPD SIEM Installation Guide

## Step 1. Copy the installation script to the server

Copy the installation script to the server:

`scopd-siem-install-online.sh`

(For example, using `scp`, `sftp`, or any other secure file transfer method.)

---

## Step 2. Server check and preparation (inside SSH)

### 2.1. Verify if the installation file is present

```bash
ls -l ~/scopd-siem-install-online.sh
```

---

### 2.2. Firewall configuration (open required ports)

<details>
  <summary>UFW configuration</summary>

If using UFW:

```bash
sudo ufw status    # check firewall state

# if enabled — open required ports:
sudo ufw allow 443/tcp      # Dashboard
sudo ufw allow 9200/tcp     # Indexer (REST)
sudo ufw allow 9300/tcp     # Indexer (cluster)
sudo ufw allow 1514/udp     # Manager (syslog)
sudo ufw allow 1514/tcp     # Manager (agent connection)
sudo ufw allow 1515/tcp     # Manager (agent registration)
sudo ufw allow 55000/tcp    # Manager (API)
sudo ufw reload
```

</details>

<details>
  <summary>firewalld configuration (rare on Ubuntu)</summary>

If using firewalld (rare on Ubuntu):

```bash
if command -v firewall-cmd >/dev/null; then
  sudo firewall-cmd --add-port=443/tcp --add-port=9200/tcp --add-port=9300/tcp \
                    --add-port=1514/udp --add-port=1515/tcp --add-port=55000/tcp \
                    --permanent
  sudo firewall-cmd --reload
fi
```

</details>

---

### 2.3. Removing old Wazuh installations (if any)

To avoid conflicts, clean up everything that might remain:

<details>
  <summary>Stop services and remove old Wazuh components</summary>

```bash
# stop services
sudo systemctl stop wazuh-manager wazuh-indexer wazuh-dashboard filebeat 2>/dev/null || true

# remove packages
sudo apt-get purge -y wazuh-manager wazuh-indexer wazuh-dashboard filebeat 2>/dev/null || true
sudo apt-get autoremove -y || true

# remove repositories and keys
sudo rm -f /etc/apt/sources.list.d/wazuh*.list /usr/share/keyrings/wazuh*.gpg

# remove directories and logs
sudo rm -rf /var/ossec \
            /etc/wazuh-indexer /var/lib/wazuh-indexer /var/log/wazuh-indexer \
            /etc/wazuh-dashboard /usr/share/wazuh-dashboard /var/log/wazuh-dashboard \
            /etc/filebeat /var/lib/filebeat /var/log/filebeat

# remove temporary installer folders
sudo rm -rf ~/wazuh-custom-install /root/wazuh-custom-install
sudo rm -f  ~/wazuh-install-files.tar /root/wazuh-install-files.tar
sudo rm -f  /var/log/wazuh-install.log

# (optional) remove forced IPv4 for apt, if it was enabled
sudo rm -f /etc/apt/apt.conf.d/99force-ipv4

# update package index
sudo apt-get update -y
```

</details>

---

## Step 3. Running the installer

Make the script executable:

```bash
chmod +x ~/scopd-siem-install-online.sh
```

Run it:

```bash
sudo bash ~/scopd-siem-install-online.sh
```

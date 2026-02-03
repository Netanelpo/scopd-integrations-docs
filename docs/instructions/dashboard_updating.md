# SIEM Dashboard Update

## Steps

1. Copy the dashboard `.deb` package to your machine.

2. Back up the Dashboard config:

   ```bash
   sudo cp /etc/wazuh-dashboard/opensearch_dashboards.yml /etc/wazuh-dashboard/opensearch_dashboards.yml.bak
   ```

3. Stop the Wazuh Dashboard service:

   ```bash
   sudo systemctl stop wazuh-dashboard
   ```

4. Reinstall the Dashboard package:

   ```bash
   sudo dpkg -i /root/wazuh-dashboard_4.12.0-1_amd64.deb
   ```

5. Restore the config from backup:

   ```bash
   sudo cp /etc/wazuh-dashboard/opensearch_dashboards.yml.bak /etc/wazuh-dashboard/opensearch_dashboards.yml
   ```

6. Start the service and check status:

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start wazuh-dashboard
   sudo systemctl status --no-pager wazuh-dashboard
   ```


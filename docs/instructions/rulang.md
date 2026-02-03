# RU Language in Dashboard SIEM

## Change Wazuh Dashboard UI language to English

1. Open the config file:

   ```bash
   sudo nano /etc/wazuh-dashboard/opensearch_dashboards.yml
   ```

2. Delete srting:

   ```yaml
   i18n.locale
   ```

3. Restart the dashboard:

   ```bash
   sudo systemctl restart wazuh-dashboard
   ```

4. Done. The UI language will switch to English after the restart.

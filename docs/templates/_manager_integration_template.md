## SIEM server configuration

Perform the following step on the SIEM server to configure the {{ integration_name }} integration.
Add the following configuration within the `<ossec_config>` block of the `/var/ossec/etc/ossec.conf` file to enable the {{ integration_name }} integration. Replace `{YOUR_API_KEY}` with your [{{ integration_name }} API key]({{ api_link }})

```md
<integration>

  <name>{{ name_tag }}</name>

  <api_key>{YOUR_API_KEY}</api_key>

  <group>{{ group_tag }}</group>

  <alert_format>json</alert_format>

</integration>
```
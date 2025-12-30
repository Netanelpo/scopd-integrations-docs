## Enable Syslog on the Manager

Add the following configuration in between the `<ossec_config>` tags of the SIEM server `/var/ossec/etc/ossec.conf` file to listen for syslog messages on TCP port 514:

```xml
<remote>
  <connection>syslog</connection>
  <port>514</port>
  <protocol>udp</protocol>
  <allowed-ips>any</allowed-ips>
</remote>
```

This enables receiving syslog messages over UDP/514 from any source.

{% include "templates/_restart_manager.md" %}

- Confirm the Syslog port is listening:

```bash
sudo ss -lunpt | grep 514
```
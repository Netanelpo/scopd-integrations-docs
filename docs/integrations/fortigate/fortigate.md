# FortiGate integration with SIEM gg

## 1. Enable Syslog/UDP reception on the SIEM Manager

Open the SIEM manager configuration file:

```md
sudo nano /var/ossec/etc/ossec.conf
````

Add the `<remote>` block (example for UDP/514):

```xml
<remote>
  <connection>syslog</connection>
  <port>514</port>
  <protocol>udp</protocol>
  <allowed-ips>any</allowed-ips>
</remote>
```

This enables receiving syslog messages over UDP/514 from any source.

Restart the SIEM manager to apply the changes:

```bash
sudo systemctl restart wazuh-manager
```

## 2. Configure Syslog/UDP forwarding on FortiGate

In the FortiGate web interface, go to:

**Log & Report → Log Settings**

1. Enable **Syslog**.
2. In the **IP** field, enter the IP address of your SIEM Manager.

You don’t need to change the port or protocol — FortiGate sends logs over **UDP/514** by default.

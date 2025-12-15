# FortiGate integration

{% include "templates/_syslog.md" %}

## Configure Syslog/UDP forwarding on FortiGate

In the FortiGate web interface, go to:

**Log & Report → Log Settings**

1. Enable **Syslog**.
2. In the **IP** field, enter the IP address of your SIEM Manager.

You don’t need to change the port or protocol — FortiGate sends logs over **UDP/514** by default.

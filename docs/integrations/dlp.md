---
rules: rules/dlp_rules.xml
local_rules: scopd_rules.xml
---

# SCOPD DLP Integration

{% include "templates/_syslog.md" %}

## DLP configuration

- In BOSS-Online open: Global settings → Complex settings → Syslog.

Specify:

udp://<SIEM_IP>:514

Where <SIEM_IP> is the public address of SIEM Manager (e.g., 45.136.16.242).

- Tick the checkboxes: - Computer events - User events

Save.

- Restart the DLP service on Windows (via services.msc, service stsrv or equivalent).

{% include "templates/_rules.md" %}

---
rules: rules/phishing_rules.xml
local_rules: scopd_rules.xml
---

# SCOPD Phishing attack simulator Integration

{% include "templates/_rules.md" %}

## The Phishing Simulator agent

!Attention! On this machine, you need both the SIEM agent installed and the Phishing Simulator client itself!

- Agent file: `/var/ossec/etc/ossec.conf`

Add one block:
```md
<ossec_config>

  <localfile>

    <log_format>syslog</log_format>

    <location>/var/log/syslog</location>

  </localfile>

</ossec_config>
```

{% include "templates/_restart_linux.md" %}

## Setting up SIEM Rules

Put the following rule definition inside the `<rules>` section in `/var/ossec/etc/rules/{{ local_rules }}`

```xml
{% include rules %}
```
{% include "templates/_restart_manager.md" %}

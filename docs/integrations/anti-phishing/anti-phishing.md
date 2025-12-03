# Wazuh integration with Anti-phishing — Step-by-step setup

## 1) Setting up the Wazuh manager (one rule)

- File: /var/ossec/etc/rules/local_rules.xml 

Insert the block:

<group name="phishing,local,">

<rule id="100180" level="12">

<decoded_as>json</decoded_as>

<if_sid>86600</if_sid>

<field name="event_type">^phishing_link_click$</field>

<description>Phishing Simulation: User clicked on a phishing link. Email: $(user_email)</description>

<options>no_full_log</options>

</rule>

</group>

- Restart the manager:

sudo systemctl restart wazuh-manager

## 2) Configuring the agent on the anti-phishing host (one log source)

!Attention! On this machine, you need both the SIEM agent installed and the Anti-phishing client itself!

- Agent file: /var/ossec/etc/ossec.conf

Add one block:

<ossec_config>

<localfile>

<log_format>syslog</log_format>

<location>/var/log/syslog</location>

</localfile>

</ossec_config>

Restart the agent

sudo systemctl restart wazuh-agent


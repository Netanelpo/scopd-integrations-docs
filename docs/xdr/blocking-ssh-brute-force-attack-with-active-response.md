## SIEM server configuration

SIEM comes with a set of default scripts used in Active Response. These scripts are located in the `/var/ossec/active-response/bin/` directory on Linux/Unix endpoints. The firewall-drop active response script works with Linux/Unix operating systems. It uses iptables to block malicious IP addresses.

1.) Open the SIEM server `/var/ossec/etc/ossec.conf` file and verify that a `<command>` block called firewall-drop with the following configuration is present within the `<ossec_config>` block:

<details>
  <summary>Firewall drop active response command</summary>

```xml
<command>
  <name>firewall-drop</name>
  <executable>firewall-drop</executable>
  <timeout_allowed>yes</timeout_allowed>
</command>
```

</details>

The `<command>` block contains information about the action to be executed on the SIEM agent:

* ```<name>```: Sets a name for the command. In this case, firewall-drop.

* ```<executable>```: Specifies the active response script or executable that must run upon a trigger. In this case, it’s the firewall-drop executable.

* ```<timeout_allowed>```: Allows a timeout after a period of time. This tag is set to yes here, which represents a stateful active response.

<details>
   <summary>Custom script option</summary>
Note You can create your own custom script to block an IP address or perform any other action.
</details>

2.) Add the <active-response> block below to the SIEM server `/var/ossec/etc/ossec.conf` configuration file:

<details>
  <summary>Active response configuration</summary>

```xml
<ossec_config>
  <active-response>
    <disabled>no</disabled>
    <command>firewall-drop</command>
    <location>local</location>
    <rules_id>5763</rules_id>
    <timeout>180</timeout>
  </active-response>
</ossec_config>
```

</details>

* ```<command>```: Specifies the command to configure. This is the command name firewall-drop defined in the previous step.

* ```<location>```: Specifies where the command executes. Using the local value means that the command executes on the monitored endpoint where the trigger event occurs.

* ```<rules_id>```: The Active Response module executes the command if rule ID 5763 - SSHD brute force trying to get access to the system fires.

* ```<timeout>```: Specifies how long the active response action must last. In this use case, the module blocks for 180 seconds the IP address of the endpoint carrying out the brute-force attack.

3. Restart the SIEM manager service to apply the changes:

```bash
sudo systemctl restart wazuh-manager
```

## Test the configuration

Perform the steps below to perform an SSH brute-force attack against the RHEL endpoint.

1.) Ping the RHEL endpoint from the Ubuntu endpoint to confirm there is network connectivity between the attacker and the victim endpoints:

```bash
ping <RHEL_IP>
```

Output

```text
PING <RHEL_IP> (<RHEL_IP>) 56(84) bytes of data.
64 bytes from <RHEL_IP>: icmp_seq=1 ttl=64 time=0.602 ms
64 bytes from <RHEL_IP>: icmp_seq=2 ttl=64 time=0.774 ms
```

2.) On the Ubuntu endpoint, install Hydra. You need Hydra to execute the brute-force attack:

```bash
sudo apt update && sudo apt install -y hydra
```

3.) On the Ubuntu endpoint, create a text file with 10 random passwords.

4.) Run Hydra from the Ubuntu endpoint to execute brute-force attacks against the RHEL endpoint using the command below. Replace `<RHEL_USERNAME>` with the username of the RHEL endpoint, `<PASSWD_LIST.txt>` with the path to the passwords file created in the previous step, and `<RHEL_IP>` with the IP address of the RHEL endpoint:

```bash
sudo hydra -t 4 -l <RHEL_USERNAME> -P <PASSWD_LIST.txt> <RHEL_IP> ssh
```

Once the attack ends, you can see on the SIEM dashboard that rule ID 5763 fired.

5.) Ping the victim endpoint from the attacker within 3 minutes of the attack execution to verify that the Active Response module has blocked the attacker's IP address:

```bash
ping <RHEL_IP>
```

Output

```text
PING 10.0.0.5 (10.0.0.5) 56(84) bytes of data.
^C
--- 10.0.0.5 ping statistics ---
12 packets transmitted, 0 received, 100% packet loss, time 11000ms
```

## Firing active response

Monitored Linux/Unix endpoints have a log file at `/var/ossec/logs/active-responses.log` where SIEM registers the active response activities. By default, the SIEM server monitors the Active Response log file. You can find the relevant section in the SIEM server `/var/ossec/etc/ossec.conf` configuration file as shown below:

<details>
  <summary>Active response log monitoring configuration</summary>

```xml
<localfile>
  <log_format>syslog</log_format>
  <location>/var/ossec/logs/active-responses.log</location>
</localfile>
```

</details>

When the active response triggers, a corresponding alert appears on the SIEM dashboard.

The alert appears because rule ID 651 is part of the default `/var/ossec/ruleset/rules/0015-ossec_rules.xml` rule file on the SIEM server. If you create a custom active response script, you must add a proper custom rule to analyze the Active Response logs that are generated.

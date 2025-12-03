# SCOPD SIEM integration with SCOPD DLP

## 1.) SIEM Manager configuration

- Open SIEM configuration:

sudo nano /var/ossec/etc/ossec.conf

- Add the block (below the existing <remote> for agents):
```md
<remote>

  <connection>syslog</connection>

  <port>514</port>

  <protocol>udp</protocol>

  <allowed-ips>0.0.0.0/0</allowed-ips>

</remote>
```

- Restart the manager:

sudo systemctl restart wazuh-manager

- Confirm the port is listening:

sudo ss -lunpt | grep 514

You should see a line with wazuh-remoted on *:514.

## 2). DLP configuration

- In BOSS-Online open: Global settings → Complex settings → Syslog.

Specify:

udp://<WAZUH_IP>:514

Where <WAZUH_IP> is the public address of Wazuh Manager (e.g., 45.136.16.242).

- Tick the checkboxes: - Computer events - User events

Save.

- Restart the DLP service on Windows (via services.msc, service stsrv or equivalent).

## 3.) SIEM Rules setup

- File

/var/ossec/etc/rules/local_rules.xml

- Edit command

sudo nano /var/ossec/etc/rules/local_rules.xml

- Content

```md
<group name="dlp,">

  <rule id="110108" level="12">

!optional     <decoded_as>dlp-syslog</decoded_as>  if a decoder is needed!

    <match>event_type="108"</match>

    <description>DLP: Clipboard with image detected</description>

    <mitre>

      <id>T1115</id>

    </mitre>

  </rule>

</group>
```

## 4.) Restart SIEMM

sudo systemctl restart wazuh-manager

sudo systemctl status wazuh-manager

## 5.) Decoder setup (OPTIONAL! It’s not required—the system works exactly the same without it. A decoder is only needed if you want to conveniently parse fields and filter by them later.)

- File

/var/ossec/etc/decoders/local_decoder.xml

- Edit command

sudo nano /var/ossec/etc/decoders/local_decoder.xml

- Content
```md
<decoder name="dlp-syslog">

  <prematch>stsrv</prematch>

</decoder>
```





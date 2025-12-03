# SCOPD SIEM integration with VirusTotal

## 1) Configure integration on the Manager

- Edit the SIEM Manager configuration:

sudo nano /var/ossec/etc/ossec.conf

- Insert the integration block right before the last </ossec_config>:
```md
<integration>

  <name>virustotal</name>

  <api_key>VIRUS_TOTAL_IP_KEY</api_key>

  <group>syscheck</group>

  <alert_format>json</alert_format>

</integration>
```
2) Define monitored folders (FIM)

Edit the SIEM Agent configuration.

- Add the directories to check inside the <syscheck> section, before </syscheck>:

Linux example:
```md
<directories check_all="yes" realtime="yes">/media/user/software</directories>
```
Windows example:
```md
file path C:\Program Files (x86)\ossec-agent

<directories check_all="yes" realtime="yes">C:\Users\Public\Downloads</directories>
```
## 3) Restart services

- Restart the Manager:

sudo systemctl restart wazuh-manager

- Restart each agent:

Linux agent:

sudo systemctl restart wazuh-agent

Windows agent (PowerShell as Administrator):

Restart-Service wazuh

## 4) Tests (with directories in paragraph 2)

- Linux tests:

1. Create folder

sudo mkdir -p /media/user/software

2. Create a simple text file (FIM should trigger):

echo test > /media/user/software/testfile.txt

3. Download the “virus” test file (VirusTotal alert expected):

sudo curl -Lo /media/user/software/suspicious-file.exe https://secure.eicar.org/eicar.com

- Windows test (PowerShell as Administrator):

Download the “virus” test file (VirusTotal alert expected):

Invoke-WebRequest -Uri "https://secure.eicar.org/eicar.com" -OutFile "C:\Users\Public\Downloads\eicar.com"

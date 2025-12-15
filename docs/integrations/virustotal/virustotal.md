---
integration_name: VirusTotal
name_tag: virustotal
group_tag: syscheck
api_link: https://docs.virustotal.com/docs/please-give-me-an-api-key
---
# VirusTotal Integration

{% include "templates/_manager_integration_template.md" %}

## Define monitored folders (FIM)

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
## Restart Services

Run the following commands as applicable:

### Restart SIEM Manager
```md
sudo systemctl restart wazuh-manager
```

### Restart Linux Agent
```md
sudo systemctl restart wazuh-agent
```

### Restart Windows Agent
(PowerShell as Administrator)
```md
Restart-Service wazuh
```

## Testing

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

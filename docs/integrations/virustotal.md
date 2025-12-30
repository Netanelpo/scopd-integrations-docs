---
integration_name: VirusTotal
name_tag: virustotal
group_tag: syscheck
api_link: https://docs.virustotal.com/docs/please-give-me-an-api-key
---
# VirusTotal Integration

{% include "templates/_integration.md" %}

## Define monitored folders (FIM)
Add the below entry within the `<syscheck>` block to monitor and alert files detected as malicious by VirusTotal.

### Linux example
file path `/var/ossec/etc/ossec.conf`
```xml
<directories check_all="yes" realtime="yes">/media/user/software</directories>
```
### Windows example:
file path `C:\Program Files (x86)\ossec-agent`
```xml
<directories check_all="yes" realtime="yes">C:\Users\Public\Downloads</directories>
```

{% include "templates/_restart_services.md" %}

Run the following commands:

## Linux Testing

Create a test directory and download the EICAR test file to trigger **File Integrity Monitoring (FIM)** and **VirusTotal enrichment**.

> ⚠️ **Expected result:**
> - FIM alert for file creation
> - VirusTotal enrichment alert

Run the following commands on the Linux agent:

```bash
sudo mkdir -p /media/user/software
sudo curl -Lo /media/user/software/suspicious-file.exe https://secure.eicar.org/eicar.com
```

## Windows Testing

Download the EICAR test file to trigger **File Integrity Monitoring (FIM)** and **VirusTotal enrichment**.

> ⚠️ **Expected result:**
> - FIM alert for file creation
> - VirusTotal enrichment alert

Run the following command in **PowerShell as Administrator**:

```powershell
Invoke-WebRequest -Uri "https://secure.eicar.org/eicar.com" `
  -OutFile "C:\Users\Public\Downloads\eicar.com"
```
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

## Alert Types

- 100 — Threat list: application launched
- 101 — Threat list: website opened
- 102 — Threat list: text input detected
- 103 — Document printed
- 104 — Copied to USB flash drive
- 105 — Copied to selected folders
- 106 — File sent (transfer)
- 107 — USB flash drive inserted
- 108 — Clipboard: image detected
- 109 — DLP: document read
- 110 — DLP: document in clipboard
- 111 — DLP: text in clipboard
- 112 — DLP: screenshot taken
- 113 — DLP: copied to USB flash drive
- 114 — DLP: copied to selected folders
- 115 — DLP: document sent
- 116 — DLP: voice/audio detected
- 117 — Anomalous behavior detected
- 118 — Hardware/software change detected
- 119 — Possible client removal detected
- 120 — Webcam: no face detected
- 121 — Webcam: unknown face detected
- 122 — Webcam: multiple faces detected
- 123 — PC shutdown delay detected
- 124 — Client issue detected
- 125 — Microphone state changed
- 126 — Critical application/website detected
- 127 — User login detected
- 128 — Blacklist: application launched
- 129 — DLP: document printed
- 130 — Linux terminal: forbidden command executed
- 131 — USB device blocked
- 132 — DLP: file in folder detected
- 133 — Clipboard: cryptocurrency address detected

# Snort Custom Rules & Detection Engineering

Custom Snort NIDS/IPS signatures, detection logic documentation, and PCAP/traffic validation reports built to detect reconnaissance, brute-force authentication attempts, unauthorized protocol usage, and suspicious payloads across segmented lab networks.

## Repository Contents

* **`local.rules`** — Production-tested custom Snort ruleset (`sid:1000001`+) written for deployment on pfSense Snort interfaces (`WAN_Net` / `LAN_Net` / `IoT_Net`). Includes both standard Snort 2 syntax and Snort 3 sticky-buffer adaptations where applicable.
* **`snort-ids-custom-rules-lab.pdf`** — Hands-on lab implementation report detailing rule syntax construction, rule ordering, false-positive tuning, and live alert verification against simulated adversary traffic from Kali Linux (`192.168.10.50`).
* **`snort-nids-detection-engineering.pdf`** — Architectural and analytical breakdown of signature-based detection engineering, payload inspection modifiers (`content`, `nocase`, `pcre`, `flags`, `threshold`), and alert triage workflows.

## Detection Coverage (`local.rules`)

The custom ruleset targets high-signal network behaviors observed during lab testing:

* **Reconnaissance & Scanning:** Detects ICMP echo sweeps, Nmap TCP SYN/NULL/Xmas scans, and banner-grabbing probes (`nc`) targeting `192.168.20.0/24`.
* **Authentication & Brute-Force Activity:** Flags repeated inbound SSH connection attempts and HTTP POST authentication floods (`hydra`) using threshold/rate-limiting (`detection_filter` / `threshold`) to reduce alert fatigue.
* **Web & Application-Layer Anomalies:** Inspects HTTP request URIs and User-Agent headers for directory traversal strings, command injection payloads, and non-standard CLI tooling (`curl`, `wget`, `python-requests`) hitting `anonsvr` (`192.168.20.50`).
* **Policy & Segmentation Violations:** Alerts on unauthorized outbound or cross-subnet traffic originating from `IoT_Net` (`192.168.30.0/24`) attempting to reach internal `LAN_Net` resources.

## Usage & Validation

1. **Deploy Rules:** Copy the contents of `local.rules` into the Snort **Custom Rules** tab under **Services > Snort > [Interface] > Rules** in pfSense (or `/etc/snort/rules/local.rules` on standalone Linux sensors).
2. **Validate Syntax:** Test rule parsing prior to restarting the sensor interface:
   ```bash
   snort -T -c /usr/local/etc/snort/snort.conf

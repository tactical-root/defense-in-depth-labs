# Defense-in-Depth Lab Series: pfSense, Snort NIDS, Host Hardening & IoT Segmentation

A four-phase network security engineering series demonstrating layered perimeter filtering, custom signature detection engineering, Linux host hardening, cross-layer log correlation, and dedicated IoT micro-segmentation inside a virtualized multi-subnet environment.

---

## Architecture Overview

The lab evolved across four integration phases from a flat two-zone perimeter into a three-tier segmented architecture governed by a central pfSense firewall and monitored by Snort NIDS:

```text
+-----------------------+        +-----------------------------------+        +-----------------------+
|  External / Admin     |        |    pfSense Firewall & Snort NIDS  |        |   Internal Server Net |
|  WAN_Net              |        |                                   |        |   LAN_Net             |
|  192.168.10.0/24      |------->|  WAN (em0): 192.168.10.1          |------->|  192.168.20.0/24      |
|                       |        |  LAN (em1): 192.168.20.1          |        |                       |
|  Kali Linux           |        |  OPT1/IOT (em2): 192.168.30.1     |        |  anonsvr (Ubuntu)     |
|  192.168.10.50        |        |                                   |        |  192.168.20.50        |
+-----------------------+        +-----------------+-----------------+        |  - Apache2 (TCP/80)   |
                                                   |                          |  - UFW + DAC Hardened |
                                                   |                          +-----------------------+
                                                   v
                                 +-----------------------------------+
                                 |  Dedicated IoT / OT Segment       |
                                 |  IoT_Net (192.168.30.0/24)        |
                                 |                                   |
                                 |  iot-node (TinyCore Linux)        |
                                 |  192.168.30.60                    |
                                 +-----------------------------------+

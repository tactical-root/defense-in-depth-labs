# Defense-in-Depth Lab Series

A progressive, multi-checkpoint network security engineering lab built in VirtualBox. This series demonstrates perimeter firewall policy enforcement, network intrusion detection, VLAN/subnet segmentation for untrusted IoT devices, and Linux host hardening across isolated virtual networks.

## Lab Topology & Network Architecture

All virtual machines run inside isolated internal VirtualBox networks routed and inspected by a central pfSense CE firewall appliance.

| Network / Segment | Interface | Subnet | Host / Node | IP Address | Role |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WAN_Net** | `em0` (WAN) | `192.168.10.0/24` | Kali Linux (`kali`) | `192.168.10.50` | External attacker / adversary simulation |
| **LAN_Net** | `em1` (LAN) | `192.168.20.0/24` | Ubuntu Server (`anonsvr`) | `192.168.20.50` | Internal production target (Apache2) |
| **IoT_Net** | `em2` (OPT1) | `192.168.30.0/24` | Tiny Core Linux (`iot-node`) | `192.168.30.60` | Constrained/untrusted IoT endpoint |
| **Gateway** | `em0`/`em1`/`em2` | `.1` on all subnets | pfSense CE | `.10.1` / `.20.1` / `.30.1` | Stateful firewall & Snort NIDS/IPS |

## Lab Modules

* **`01-pfsense-snort-cross-layer-defense.pdf`**
  * Deployed pfSense CE with segregated `WAN_Net` and `LAN_Net` internal switches.
  * Configured stateful firewall rules allowing baseline HTTP traffic to `anonsvr` (`192.168.20.50`) while explicitly blocking external SSH access.
  * Deployed and configured Snort on pfSense interfaces to generate real-time alerts alongside firewall state logs, validating cross-layer visibility with `curl`, `nc`, and packet captures.
* **`02-pfsense-iot-network-segmentation.pdf`**
  * Provisioned `IoT_Net` (`192.168.30.0/24`) on `em2` and integrated a lightweight Tiny Core Linux node (`iot-node`, `192.168.30.60`).
  * Engineered strict inter-VLAN/inter-subnet firewall policies preventing lateral movement from `IoT_Net` into `LAN_Net` (`192.168.20.0/24`) while permitting controlled outbound telemetry/updates.
  * Verified isolation boundaries and rule ordering using ICMP (`ping`), TCP port probing (`nc`), and HTTP requests.
* **`03-linux-host-hardening-and-layering.pdf`**
  * Implemented defense-in-depth host controls on `anonsvr` (Ubuntu Server) to backstop network perimeter defenses.
  * Hardened SSH daemon configuration, enforced host-based firewall rules (`ufw`), audited listening services/ports, and applied principle-of-least-privilege file and service permissions.
  * Tested multi-layered resilience against brute-force and enumeration attempts from `192.168.10.50`.

## Core Tools & Technologies

* **Virtualization & OS:** Oracle VirtualBox, pfSense CE, Kali Linux, Ubuntu Server, Tiny Core Linux
* **Network & Host Security:** Snort NIDS/IPS, pfSense Packet Filter (`pf`), UFW, OpenSSH
* **Testing & Validation:** `nmap`, `hydra`, `curl`, `wget`, `nc` (Netcat), `tcpdump` / PCAP analysis

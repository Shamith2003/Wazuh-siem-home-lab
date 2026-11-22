# 🛡️ Wazuh SIEM & File Integrity Monitoring (FIM) Home Lab

A hands-on security monitoring lab demonstrating the deployment of a *Wazuh SIEM (Security Information and Event Management)* environment on Ubuntu Server and integrating a Windows endpoint agent for real-time log collection and *File Integrity Monitoring (FIM)*.

---

## 🏗️ Lab Architecture

| Component | Host Environment | Role |
| :--- | :--- | :--- |
| *Wazuh Manager & Indexer* | Ubuntu Server (VMware Workstation  — Bridged Adapter) | Collects, indexes, analyzes, and stores security events from enrolled agents |
| *Wazuh Agent* | Windows Host Machine | Forwards system logs and real-time file system events to the Wazuh Manager |

---

## 🚀 Implementation Walkthrough

### 1. Wazuh Manager Installation (Ubuntu Server)
- Configured VirtualBox networking with a *Bridged Adapter* so the Ubuntu VM and Windows host reside on the same local subnet.
- Imported the official Wazuh GPG signing key and executed the all-in-one interactive installer:
  <br>
  <br>
  curl -s [https://packages.wazuh.com/key/GPG-KEY-WAZUH](https://packages.wazuh.com/key/GPG-KEY-WAZUH) | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg<br>
  curl -sO [https://packages.wazuh.com/4.12/wazuh-install.sh](https://packages.wazuh.com/4.12/wazuh-install.sh) && sudo bash ./wazuh-install.sh -a -i
  <br>
  <br>
- Accessed the Wazuh Web Dashboard via https://<ubuntu-vm-ip> using the generated administrative credentials.

### 2. Windows Agent Onboarding & Authentication
- Installed the Wazuh Windows Agent (.msi) package on the Windows host.
- Registered the endpoint on the Ubuntu manager via manage_agents and extracted the authentication key:
  bash
  sudo /var/ossec/bin/manage_agents
  
- Configured the Manager IP and authentication key inside the *Wazuh Agent Manager GUI* and restarted the agent service.

### 3. Configuring Real-Time File Integrity Monitoring (FIM)
- Edited the agent configuration file (C:\Program Files (x86)\ossec-agent\ossec.conf) under the <syscheck> directory block to monitor a target directory in real time:
  xml
  <directories realtime="yes">C:\Users\abc\Test</directories>
  
- Restarted the Wazuh agent service to apply the updated policy.

### 4. Verification & Alert Validation
- Verified that the Windows agent appeared as *Active* in the Wazuh Dashboard.
- Created, modified, and deleted test files inside the monitored directory and confirmed real-time FIM alerts triggered on the dashboard.

---

## 🛠️ Technologies & Skills Demonstrated
- *SIEM & Log Management:* Wazuh Manager, Wazuh Indexer, Dashboard Alert Triage
- *Endpoint Security:* Wazuh Agent deployment, key-based agent registration, Syscheck FIM configuration (ossec.conf)
- *Systems & Networking:* Ubuntu Server administration, Windows administration, VirtualBox Bridged Networking

---

## 📚 Reference
- Lab walkthrough based on the Wazuh Home Lab — SIEM and File Integrity Monitoring guide by Royden Rebello.

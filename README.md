# Azure SOC & Honeynet: Live Attack Detection & Remediation

> A hands-on security operations lab that deploys a honeynet in Azure exposed to live attack traffic, monitors it with Microsoft Sentinel, then hardens the environment and measures the reduction in security incidents.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Objectives](#-objectives)
- [Architecture](#-architecture)
- [Attack Maps Before Hardening](#-attack-maps-before-hardening--security-controls)
- [Metrics Before Hardening](#-metrics-before-hardening--security-controls)
- [Attack Maps After Hardening](#-attack-maps-after-hardening--security-controls)
- [Metrics After Hardening](#-metrics-after-hardening--security-controls)
- [Conclusion](#-conclusion)
- [Author](#-author)

---

## 📌 Overview

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. Security metrics were measured in the insecure environment for 24 hours, security controls were applied to harden the environment, and metrics were measured for another 24 hours to demonstrate the effectiveness of the controls.

![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

---

## 🎯 Objectives

- Deploy a deliberately insecure Azure environment to attract real-world attack traffic
- Ingest logs from multiple sources into a centralized Log Analytics workspace
- Configure Microsoft Sentinel to generate attack maps, alerts, and incidents
- Measure security metrics before and after applying hardening controls
- Demonstrate the measurable impact of security controls on reducing incidents

---

## 🔷 Architecture

### Components

| Component | Purpose |
|-----------|---------|
| **Virtual Network (VNet)** | Network infrastructure for all resources |
| **Network Security Group (NSG)** | Traffic filtering and boundary protection |
| **Virtual Machines (2 Windows, 1 Linux)** | Honeynet targets exposed to live traffic |
| **Log Analytics Workspace** | Centralized log ingestion and querying |
| **Azure Key Vault** | Secrets management |
| **Azure Storage Account** | Data storage |
| **Microsoft Sentinel** | SIEM for alerts, incidents, and attack maps |

### Metrics Tracked

- `SecurityEvent` -- Windows Event Logs
- `Syslog` -- Linux Event Logs
- `SecurityAlert` -- Log Analytics Alerts Triggered
- `SecurityIncident` -- Incidents created by Sentinel
- `AzureNetworkAnalytics_CL` -- Malicious flows allowed into the honeynet

### Before Hardening

For the BEFORE metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the internet with no use of Private Endpoints.

![Architecture Before](https://i.imgur.com/aBDwnKb.jpg)

### After Hardening

For the AFTER metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

![Architecture After](https://i.imgur.com/YQNa9Pp.jpg)

---

## 🌐 Attack Maps Before Hardening / Security Controls

![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)

---

## 📊 Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment over 24 hours.

**Start Time:** 2023-03-15 17:04:29
**Stop Time:** 2023-03-16 17:04:29

| Metric | Count |
|--------|-------|
| SecurityEvent | 19,470 |
| Syslog | 3,028 |
| SecurityAlert | 10 |
| SecurityIncident | 348 |
| AzureNetworkAnalytics_CL | 843 |

---

## 🔒 Attack Maps After Hardening / Security Controls

All map queries returned no results due to no instances of malicious activity during the 24-hour period after hardening.

---

## 📊 Metrics After Hardening / Security Controls

The following table shows the metrics measured after security controls were applied over another 24 hours.

**Start Time:** 2023-03-18 15:37
**Stop Time:** 2023-03-19 15:37

| Metric | Count |
|--------|-------|
| SecurityEvent | 8,778 |
| Syslog | 25 |
| SecurityAlert | 0 |
| SecurityIncident | 0 |
| AzureNetworkAnalytics_CL | 0 |

### Improvement Summary

| Metric | Before | After | Reduction |
|--------|--------|-------|-----------|
| SecurityEvent | 19,470 | 8,778 | 55% |
| Syslog | 3,028 | 25 | 99% |
| SecurityAlert | 10 | 0 | 100% |
| SecurityIncident | 348 | 0 | 100% |
| AzureNetworkAnalytics_CL | 843 | 0 | 100% |

---

## ✅ Conclusion

A mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Security metrics were measured before and after applying hardening controls, demonstrating a dramatic reduction in security events and incidents. Security alerts and malicious network flows were eliminated entirely following the implementation of controls, confirming their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, more security events and alerts may have been generated within the 24-hour period following the implementation of security controls.

---

## 👤 Author

**Jarred Ward**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-blue?logo=linkedin&style=for-the-badge)](https://www.linkedin.com/in/jarredward1/)

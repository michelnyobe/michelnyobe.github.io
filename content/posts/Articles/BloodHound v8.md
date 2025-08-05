---
title: "BloodHound v8"
date: 2025-08-03
draft: false
tags: ["Active Directory", "BloodHound", "SpecterOps", "Red Team", "Cybersecurity", "Reconnaissance", "Neo4j"]
categories: ["Tools", "Offensive Security", "AD Enumeration"]
summary: "Explore BloodHound v8, an advanced Active Directory mapping tool used by Red Teams to identify attack paths. Discover its setup, graph-based architecture, and new security and management features."
showToc: true
tocOpen: true
---

## **Introduction**

BloodHound v8.0, released on **July 29, 2025**, marks one of the most significant functional updates since the tool's inception. With this version, SpecterOps introduces a major evolution in identity attack path management, expanding BloodHound’s capabilities beyond Active Directory and Entra ID thanks to OpenGraph.

## **1. Ease of Use: Faster and Clearer Access to Information**

### Table View  
Users can now select the **Table** layout to display query results in a sortable, structured table, allowing them to choose relevant data and export it to CSV. This enhances quick analysis and facilitates collaboration between identity and security teams.

### Query Library  
Over **170 prebuilt queries**, organized in YAML format, are available in both BloodHound Enterprise and Community Editions. These queries cover practical use cases and are accessible through a dedicated interface.
![Texte alternatif](images/20250805123933.png])

## **2. ACE Inheritance Tracking**  

BloodHound can now precisely highlight **where privileges are inherited** in Active Directory, allowing defenders to mitigate risk at the source without manually tracing permission inheritance.

## **3. Microsoft Privileged Identity Management (PIM)**  

BloodHound Enterprise now supports PIM roles: it detects attack paths that rely on these roles—even when they are not active—and flags misconfigurations where sensitive roles can be activated **without MFA**.

## **4. Expanded Visibility into Domain and Forest Trusts**  

The previous TrustedBy model has been replaced by **four new trust edge types**, offering more accurate visibility into how trusts are configured and whether they can be abused.

## **5. BloodHound OpenGraph: A New Frontier in Attack Path Analysis**

OpenGraph enables BloodHound to ingest data from third-party platforms (GitHub, Snowflake, Microsoft SQL Server, etc.), extending attack path mapping beyond AD to any critical enterprise system.

## **6. New Extensibility-Oriented Features**

### Privilege Zones Analysis  
Allows the definition of "Privilege Zones" to focus on the most sensitive or regulated assets (e.g., PCI, HIPAA). This enables a refined, organization-wide Least Privilege approach.

### Integrations  
- **ServiceNow**: Automatically generates tickets to track discovered vulnerabilities.  
- **Duo**: Supports multi-factor authentication for accessing BloodHound Enterprise.

## **Conclusion**

BloodHound v8.0 represents a major step forward by combining:

- **Improved usability** (Table View, Query Library, Inheritance Tracking)  
- **Extended reach** (OpenGraph for external platforms)  
- **Stronger security posture** (PIM support, Privilege Zones, MFA integrations)  

Whether you're part of a **Red Team** mapping complex attack paths or a **Blue Team** managing identity risk, BloodHound v8.0 is a strategic tool for proactively and holistically managing access and privilege across the enterprise.

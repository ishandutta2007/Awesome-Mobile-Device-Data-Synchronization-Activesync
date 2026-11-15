# Awesome-Mobile-Device-Data-Synchronization-Activesync

# Awesome Mobile Device Data Synchronization (ActiveSync)

> A curated list of top Mobile Device Data Synchronization (ActiveSync) platforms — both SaaS/Hosted commercial solutions and open-source alternatives for syncing mail, contacts, and calendars between mobile devices and backend servers.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [Microsoft Exchange ActiveSync](#microsoft-exchange-activesync)
  - [Google Sync](#google-sync)
  - [IBM Traveler](#ibm-traveler)
  - [VMware Workspace ONE Boxer](#vmware-workspace-one-boxer)
  - [SyncGene](#syncgene)
  - [AkrutoSync](#akrutosync)
- [Open-Source Software](#-open-source-software)
  - [Z-Push](#z-push)
  - [Tolk](#tolk)
  - [grommunio-sync](#grommunio-sync)
  - [sync•gw](#syncgw)
  - [EverSync](#everysync)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted Mobile Device Data Synchronization platforms. These are not open-source but are included as reference points for feature comparison.

### Microsoft Exchange ActiveSync
**Type:** Native EAS protocol server for Exchange
Exchange ActiveSync is the canonical implementation of the EAS protocol, providing push email, contacts, and calendar sync between Exchange Server/Exchange Online and mobile devices. It is the protocol that all open-source implementations aim to interoperate with, supporting device management, remote wipe, and policy enforcement.

### Google Sync
**Type:** Google Workspace sync via EAS and native protocols
Google Sync historically provided EAS support for Gmail, Calendar, and Contacts, though Google has been reducing EAS support in favor of native protocols and OAuth. It remains a reference point for organizations using Google Workspace with legacy device requirements.

### IBM Traveler
**Type:** Enterprise mobile sync for Domino/Notes
IBM Traveler (formerly Lotus Traveler) provides mobile email, calendar, and contacts sync for IBM Domino environments, supporting EAS-compatible devices. It is positioned for organizations with existing Domino infrastructure needing mobile access.

### VMware Workspace ONE Boxer
**Type:** Secure mobile email client with EAS backend
Boxer is a mobile email client that connects to EAS servers, with containerization, security policies, and integration into Workspace ONE UEM. It is positioned for enterprises wanting a managed email app with EAS compatibility.

### SyncGene
**Type:** Cloud-to-cloud sync service (not EAS server)
SyncGene provides synchronization between cloud services (Google, Microsoft 365, iCloud) without requiring an EAS server. **Important distinction:** It is a sync service rather than an EAS implementation—it syncs data between accounts, not between a device and a server via the EAS protocol.

### AkrutoSync
**Type:** Windows desktop sync for Outlook
AkrutoSync syncs Outlook data (contacts, calendars, tasks, notes) to phones over Wi-Fi or the internet without using a cloud service. It is positioned for users wanting to sync Outlook without Exchange or a third-party cloud.

---

## 🟩 Open-Source Software

Self-hosted, community-driven ActiveSync implementations. The open-source ecosystem spans **mature IMAP-to-EAS gateways**, **modern JMAP-to-EAS bridges**, **full groupware suites with EAS interfaces**, and **portable sync servers**.

### Z-Push
**Stack:** PHP | **License:** GPL-3.0
Z-Push is the most established open-source implementation of the Microsoft ActiveSync protocol, explicitly designed to synchronize emails (including push notifications), personal contacts, and calendar items between a central HTTP server and a mobile device . Key characteristics:
- **Broad backend support**: Works with IMAP, CalDAV, and CardDAV servers, making it compatible with a wide range of mail and groupware stacks 
- **Widely deployed**: Used by Mail-in-a-Box and other self-hosted mail platforms for iOS push email support [citation:8]
- **Proven track record**: Referenced as the standard that newer projects (like Tolk) compare themselves against [citation:1]

**Trade-off:** Development has slowed in recent years, and the PHP codebase is aging. It remains the most battle-tested option for IMAP-to-EAS bridging.
**Best for:** Organizations with existing IMAP/CalDAV/CardDAV infrastructure wanting to add EAS support for mobile devices, accepting a mature but slower-moving codebase.

### Tolk
**Stack:** Go / SQLite / JMAP | **License:** Open Source (per repository)
Tolk is a modern EAS server that sits in front of a **JMAP mail server** (such as Stalwart), doing for JMAP what Z-Push does for IMAP [citation:1]. Key characteristics:
- **Translates EAS to JMAP**: Phones add an "Exchange" account and get mail, contacts, and calendar with push, while everything lives on a standards-based JMAP server
- **Device state management**: Keeps no mail; translates each EAS command into JMAP calls with the device's own credentials, remembering per-device SyncKeys and item IDs in SQLite
- **Broad client support**: Developed against Gmail on Android and Stalwart 1.0; iOS Mail, Samsung Email, and Outlook are implemented but not yet tested
- **Full feature set**: Mail (folders, sync windows, attachments, drafts, out-of-office), contacts (JSContact), calendars (JSCalendar) with recurrence and invitations, shared mailboxes, push via Ping/hanging Sync, Autodiscover, and device management (list, block, wipe)
- **Single static binary**: Runs as one container behind a TLS reverse proxy with no shell or distribution in the image

**Trade-off:** Early-stage (version 0.1), with some clients implemented but not yet tested.
**Best for:** Organizations running JMAP-based mail servers (like Stalwart) wanting EAS support for mobile devices without deploying a full groupware suite.

### grommunio-sync
**Stack:** PHP | **License:** Open Source
grommunio-sync is the EAS (Exchange ActiveSync) interface for the grommunio groupware platform [citation:3]. It provides the mobile synchronization layer for grommunio's Exchange-compatible stack, enabling Android, iOS, and Outlook clients to sync mail, contacts, and calendars.
**Best for:** Organizations using grommunio as their groupware platform wanting native EAS support for mobile devices.

### sync•gw
**Stack:** PHP | **License:** GPL-3.0
sync•gw is a portable server software providing synchronization between mobile devices and a private server, supporting **EAS 2.5 through 16.1** alongside WebDAV (CalDAV/CardDAV) [citation:5]. Key characteristics:
- **No binary dependencies**: Runs on any web server with PHP—no additional software or tools required
- **Multi-protocol**: Supports XML and WBXML, EAS, and WebDAV protocols
- **Feature coverage**: Contacts, calendar, tasks, notes, and experimental mail synchronization
- **Simple setup**: Composer install, define admin password, connect database, and it is ready for first sync
- **Internationalization**: Full support for multi-byte languages (e.g., Japanese) and time zones

**Trade-off:** Small community (2 stars on the main repository) and experimental mail support.
**Best for:** Small deployments wanting a lightweight, portable PHP-based sync server supporting both EAS and WebDAV, accepting a smaller ecosystem.

### EverSync
**Stack:** Android (Kotlin) | **License:** Open Source (per repository)
EverSync is a free, open-source **universal sync hub for personal data** on Android, syncing contacts and calendars across Google, iCloud, Nextcloud, and custom CalDAV/CardDAV servers [citation:2]. Key characteristics:
- **Multi-provider support**: Google, iCloud, Nextcloud, and any CardDAV/CalDAV server
- **Health Connect bridge**: Syncs fitness data
- **Background periodic sync**: Via WorkManager, rescheduled on boot/update
- **Encrypted credential storage**: Credentials kept in app-private encrypted storage
- **OAuth2 PKCE**: Via Custom Tabs, with no client secrets shipped

**Important distinction:** EverSync is a **client-side sync app** that syncs between cloud accounts and the device—it is not an EAS server implementation. It addresses the need for multi-provider contact/calendar sync without deploying server infrastructure.
**Best for:** Android users wanting to sync contacts and calendars across multiple cloud providers without an EAS server or groupware platform.

---

## 📊 Comparison at a Glance

| Software | License | Stack | EAS Server | Backend Support | Self-Hosted | Best For |
|----------|---------|-------|------------|-----------------|-------------|----------|
| **Microsoft Exchange** | Proprietary | On-prem/Cloud | ✅ Native | Exchange | (On-prem) | Native EAS environment |
| **Z-Push** | GPL-3.0 | PHP | ✅ | IMAP, CalDAV, CardDAV | ✅ | Proven IMAP-to-EAS bridge |
| **Tolk** | Open Source | Go/SQLite | ✅ | JMAP | ✅ | Modern JMAP-to-EAS bridge |
| **grommunio-sync** | Open Source | PHP | ✅ | grommunio | ✅ | grommunio groupware |
| **sync•gw** | GPL-3.0 | PHP | ✅ | IMAP, CalDAV, CardDAV | ✅ | Lightweight portable sync |
| **EverSync** | Open Source | Android | ❌ (Client) | Google, iCloud, Nextcloud | ✅ (App) | Multi-provider client sync |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source Mobile Device Data Synchronization (ActiveSync) tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source.

**Note on the open-source ecosystem:** ActiveSync implementations in open source are **dominated by Z-Push** as the long-standing standard for bridging IMAP/CalDAV/CardDAV to EAS . The newer **Tolk** addresses a modern gap—JMAP servers like Stalwart lack EAS support, and Tolk fills that void with a clean Go implementation [citation:1]. **grommunio-sync** serves the grommunio ecosystem specifically [citation:3], while **sync•gw** offers a portable PHP alternative with WebDAV support [citation:5]. On the client side, **EverSync** provides multi-provider contact/calendar sync without requiring an EAS server at all [citation:2]. The original **SynCE** project for Windows Mobile devices is archived and should not be considered for new deployments [citation:6].

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐

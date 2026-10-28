# Privacy Policy

<p align="center">
  <img src="./assets/logo.png" alt="PacketCircle" width="140" />
</p>

**Effective date:** 2 August 2026  
**Applies to:** PacketCircle for **iOS**, **macOS**, **Linux** and **Windows** (Native apps)  
**Publisher:** Walter Hofstetter / **netwho**

This policy is written for users and for App Store review. PacketCircle has **nothing to hide**: there is **no backend that receives your captures**, and there is **no analytics, advertising, or tracking SDK** in the apps.

---

## App Store “App Privacy” summary

For Apple’s App Privacy labels, PacketCircle is intended to be declared as:

> **Data Not Collected**

Meaning: the developer does **not** collect data from the app for tracking, advertising, analytics, or our own servers. Processing happens **on your device**.

---

## What PacketCircle does **not** do

- Does **not** require an account or login  
- Does **not** collect, upload, or transmit your PCAP / PCAPNG files to netwho or any third-party analytics/advertising service  
- Does **not** include third-party ad, analytics, crash-reporting, or tracking SDKs  
- Does **not** sell personal information  
- Does **not** perform **live network capture / sniffing on iOS** (App Store iOS builds analyze files you open)

---

## What the App processes **on your device**

Depending on how you use PacketCircle, the App may read and process **locally**:

| Data | Where | Purpose |
|------|--------|---------|
| **Capture files** you open (PCAP / PCAPNG), including addresses, ports, protocols, and payload bytes in those files | On device | Circle, gauges, talkers, session health, decode, Follow TCP Stream, export you initiate |
| **Live capture** from a network interface you select | **macOS, Linux and Windows only**, on device | Optional live analysis on the desktop |
| **App preferences** (Options, panel layout, tour “seen” flags, etc.) | On device (`UserDefaults` / app container) | Remember your settings |

Network captures can contain sensitive or personal data. **You** choose which files to open, store, share, or delete, and you remain responsible for complying with applicable law.

---

## iOS vs macOS (important for review)

| | **iOS (App Store)** | **macOS** |
|--|---------------------|-----------|
| Input | Open PCAP / PCAPNG from Files, AirDrop, share sheet, document browser | Open files **and** optional live capture |
| Processing | On device | On device |
| Developer servers | None for capture analysis | None for capture analysis |

---

## How information is used

On-device processing exists only to provide App features, for example:

- visualizing communication pairs on the circle  
- traffic statistics and native TCP session-quality estimates  
- decoding frames and following TCP streams  
- saving or exporting a capture **when you choose to**

We do **not** use your captures to train models, build profiles, or target ads.

---

## Sharing

- We do **not** sell your data.  
- The App does **not** upload your capture files to our servers.  
- If **you** use system features (AirDrop, Files, Mail, Messages, opening a file in another app on Mac, iCloud Drive, etc.), those actions are initiated by you and governed by Apple’s / that product’s terms and privacy policy.

Apple may collect App Store, TestFlight, or diagnostic information according to **your** device settings and [Apple’s Privacy Policy](https://www.apple.com/legal/privacy/). That collection is by Apple, not by PacketCircle’s analysis engine.

---

## Retention & deletion

- Capture files and preferences stay on your device (or in locations you choose) until **you** delete them.  
- Uninstalling the App removes data in the App’s container.  
- Files you saved elsewhere (Files app, Downloads, iCloud Drive, …) remain until you delete them there.

---

## Children

The App is not directed at children under 13 (or the minimum age required in your country).

---

## Changes

We may update this Privacy Policy. When we do, we revise the **effective date** at the top of this page.

---

## Contact

Privacy questions: [privacy@netwho.com](mailto:walter.hofstetter@netwho.com)  

Walter Hofstetter / netwho  

**Canonical policy URL (for App Store Connect):**  
https://github.com/netwho/PacketCircle-Native-Docs/blob/main/docs/PRIVACY.md

---

*Made with love for the packet community.*

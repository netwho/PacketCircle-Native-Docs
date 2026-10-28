# PacketCircle for iPhone and iPad — Installation

<p align="center"><img src="../assets/ios-readme-hero.jpg" alt="PacketCircle on iOS" width="320" /></p>

PacketCircle for iOS is a native SwiftUI offline analyzer with the shared Rust core. It opens captures you already have on the device and does **not** capture live traffic.

**Quick links:** [Quickstart](./QUICKSTART.md) · [User manual](./USER-MANUAL.md) · [Detailed usage](./DETAILED-USAGE.md) · [Limitations](./LIMITATIONS.md) · [Master document](../PACKETCIRCLE-NATIVE.md)

## 1. Requirements

| | |
|---|---|
| **OS** | iOS / iPadOS 17 or newer |
| **Device** | iPhone or iPad. Roughly 50–200 MB captures are comfortable |
| **Network** | Not needed to analyze — analysis stays on the device |

## 2. Install

1. Open the **App Store** and search for **PacketCircle** (publisher: Walter Hofstetter / netwho), or use the link from the project page.
2. Tap **Get**. The app is free, with no in-app purchases.

Pre-release builds may be available via **TestFlight** when announced.

## 3. Get a capture onto the device

| Method | How |
|---|---|
| **Files app** | Save the `.pcap` / `.pcapng` (or `.gz`, `.cap`, `.caz`) to iCloud Drive or On My iPhone, then open it from PacketCircle's **Open** button |
| **Share sheet** | In Mail, Messages, Safari or another app, tap the capture → **Share** → **PacketCircle** |
| **AirDrop** | AirDrop the file from a Mac; choose **PacketCircle** |
| **Demo** | Nothing to import — use the built-in demo capture from the empty canvas |

## 4. First launch

The empty canvas offers the guided onboarding and the demo loop. Open a capture and PacketCircle draws the Circle; swipe between **Circle**, **Gauges**, **Talkers** and **Decode** and open **Options** for settings. Continue with the [Quickstart](./QUICKSTART.md).

## 5. Updating and removing

Updates arrive through the App Store. Deleting the app removes its settings and any captures stored in its container; files you opened from Files stay where they are.

## 6. Troubleshooting

| Symptom | Fix |
|---|---|
| App closes on a large file | iOS terminates apps that use too much memory; use a smaller capture or filter it on a desktop first |
| File will not open | Check the format (PCAP, PCAPNG, gzip, Sniffer `.cap`/`.caz`); convert others with `editcap` |
| Cannot find the capture in Files | Move it to iCloud Drive or On My iPhone first |
| Want live capture | Use the macOS, Linux or Windows app |

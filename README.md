# PacketCircle Native Docs

<p align="center">
  <img src="./docs/assets/logo.png" alt="PacketCircle" width="180" />
</p>

PacketCircle Native provides a native **iOS + macOS** UI for exploring network communication pairs from PCAP/PCAPNG captures. It is inspired by (and conceptually evolved from) the original open-source [PacketCircle Wireshark plugin](https://github.com/netwho/PacketCircle).

**PacketCircle is not a mobile Wireshark.** It contains **no open-source Wireshark / libwireshark dissection**. Decodes are native and limited: you get reasonable IP and TCP detail and some application-layer recognition, but they are **not comparable** to Wireshark’s dissectors.

<p align="center">
  <img src="./docs/assets/ios-readme-hero.jpg" alt="PacketCircle iOS — Services map" width="360" />
</p>

<p align="center"><em>Services view — hosts on the left, applications on the right, edges colored by protocol.</em></p>

**Free to use** · **not GPL / not open source** — see [License](./docs/LICENSE.md).  
We do **not** collect ANY data — see [Privacy](./docs/PRIVACY.md).

## Docs

### iOS
- **[User Manual](./docs/ios/USER-MANUAL.md)** — background, functions, options, workflows, and current Simulator screenshots
- [Quickstart](./docs/ios/QUICKSTART.md)
- [Detailed usage](./docs/ios/DETAILED-USAGE.md) — representations, analyst workflows, Talkers multi-select
- [Limitations](./docs/ios/LIMITATIONS.md)
- **[Release notes 1.1.0](./docs/ios/RELEASE-NOTES-1.1.0.md)** — conversation UX, Gauges, host names, decode depth
- **[Known issues 1.0.0](./docs/ios/KNOWN-ISSUES-1.0.0.md)** — archive (resolved in 1.1.0)

> iOS **1.1.0** continues the conversation-first triage story (App Store availability depends on review timing).

### macOS
- **[User Manual](./docs/macos/USER-MANUAL.md)** — background, functions, options, workflows, and screenshots
- [Quickstart](./docs/macos/QUICKSTART.md)
- [Detailed usage](./docs/macos/DETAILED-USAGE.md) — analyst workflows
- [Limitations](./docs/macos/LIMITATIONS.md)

> macOS **1.1.0 Tech Preview** — desktop circle/table/gauges analysis, live capture, and Wireshark handoff.

### Legal
- [License](./docs/LICENSE.md)
- [Privacy Policy](./docs/PRIVACY.md)

---

*Made with love for the packet community.*

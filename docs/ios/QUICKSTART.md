# PacketCircle iOS: Quickstart

<p align="center">
  <img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

PacketCircle iOS is an offline analyzer for PCAP/PCAPNG. Open a capture, explore talkers on a **Hosts** ring or a **Services** map, triage TCP health on **Gauges**, then drill into session health, decode, and Follow TCP Stream.

## Screenshots

| Home | Hosts circle | Services map |
|:---:|:---:|:---:|
| ![Home](../assets/ios-home.jpg) | ![Circle](../assets/ios-circle.jpg) | ![Services](../assets/ios-services.jpg) |

| Gauges | Follow TCP Stream | Decode |
|:---:|:---:|:---:|
| ![Gauges](../assets/ios-gauges.jpg) | ![Follow TCP Stream](../assets/ios-follow-tcp-stream.jpg) | ![Decode](../assets/ios-decode.jpg) |

## Requirements

- iOS 17+ (device or Simulator)
- Offline PCAP/PCAPNG only — **no live capture** on device

## 1) Open a capture

1. Open PacketCircle iOS.
2. Tap **Open Capture** and pick a `*.pcap` / `*.pcapng` (or use **Demo Mode** / the **guided tour**).
3. The app loads / analyzes the entire trace.
4. The bottom status bar shows: `<file> · <X pkts> · <Y pairs>`

## 2) Explore

- **Circle** — Hosts ring (who↔whom), **Services** map (host→HTTP/SSH/…) or **Networks** (hosts grouped by network; tap the layout button to cycle). Color by **Protocol** or **Quality**. Optional **Show host names** in Options.
- **Gauges** — rates, top talkers/protocols, **Degraded Quality Conversations**, and a **quality timeline** (tap → time slice → Talkers).
- **Talkers** — **all** analyzed conversations (Circle keeps Top‑N for the ring); check rows to filter / Decode / Save PCAP.
- **Decode** — packet list + details / hex (friendly protocol labels; native depth).

## 3) Replay (optional)

Tap the bottom capture status bar → confirm **Replay** for original timing.

## 4) Session details

Select a pair → **Session details**. The conversation header is bi-directional. **Whole conversation** shows no ports; pick a **socket** for `TCP/80 HTTP`-style labels on both sides. Then TCP health, charts, exchange ladder, application / TLS / ICMP previews.

## 5) Follow TCP Stream

From Session details, open **Follow TCP Stream** for capped ASCII/hex reassembly (client = red, server = blue). Adjust the budget under **Options** (gear) if needed.

## Next

Read **[Detailed usage](./DETAILED-USAGE.md)** for representations, Gauges quality panels, multi-select Talkers workflows, and Options.  
See the **[Release history](../RELEASES.md)** for what changed in each version (current 1.3.8).

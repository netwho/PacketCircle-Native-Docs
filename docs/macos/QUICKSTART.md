# PacketCircle macOS — Quickstart

<p align="center">
<img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

*Tech Preview build. See the [User Manual](USER-MANUAL.md) for full detail.*

## Install

1. Download `PacketCircle-<version>.dmg` (Tech Preview).
2. Open the DMG, drag **PacketCircle** into **Applications**.
3. Launch it from Applications (or Spotlight).

The Tech Preview build is signed with a Developer ID and notarized, so it should open normally without a Gatekeeper warning.

## First run

| Step | Action |
|---|---|
| 1 | **File → Open Capture…** (`⌘O`) to load a PCAP/PCAPNG you already have. |
| 2 | Or **File → Demo Mode** (`⌥⌘D`) to load a bundled sample capture and look around immediately. |
| 3 | Or **File → Interfaces…** (`⇧⌘I`) to capture live from a local interface instead. |

Once something is loaded, the **Circle** view is the default: hosts on a ring, conversations as arcs. Switch to **Table** or **Gauges** from the view-options bar, and open **Decode** (`⇧⌘D`) whenever you need packet-level detail.

## Requirements

- macOS 14 or later, Apple Silicon.
- Wireshark installed locally, only if you want to use the **Open in Wireshark** handoff.

## Next

- [User Manual](USER-MANUAL.md) — every screen and control, in detail.
- [Detailed usage](DETAILED-USAGE.md) — analyst workflows.
- [Limitations](LIMITATIONS.md) — what this is (and isn't).

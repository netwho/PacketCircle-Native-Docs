# PacketCircle iOS — Release notes 1.2.4

<p align="center">
  <img src="../assets/logo.png" alt="PacketCircle" width="140" />
</p>

**Release:** **1.2.4** (App Store build **20**)  
**Follows:** [Release notes 1.1.0](./RELEASE-NOTES-1.1.0.md)

1.2.4 deepens capture and decode (IGMP/MLD, TLS on alt HTTP ports, PCAP-over-IP client, payload NetFlow/syslog, richer mDNS) and polishes live-stream UX — still **offline-first** on iPhone, still **not** mobile Wireshark.

---

## Highlights

| Area | What you get |
|------|----------------|
| **Multicast** | IGMP / MLD decode — groups, queries, reports, well-known labels |
| **TLS** | HTTPS on common alt ports (8080, 80xx, 8888) when the payload is TLS |
| **PCAP over IP** | Optional client to a remote classic-PCAP stream (`TCP@host:port`) |
| **Collectors** | Payload NetFlow / syslog on custom UDP ports (not only 2055/514) |
| **mDNS** | Richer Bonjour service types, cache-flush / QU, extra A/AAAA |
| **Live stream** | Incremental updates without re-packing the Circle every second |

---

## What's new

### Decode & classification

- **IGMP / MLD** — group addresses, v1/v2/v3 queries and reports (source lists, INCLUDE/EXCLUDE), SSDP / mDNS / all-systems labels where applicable.
- **TLS on HTTP alt ports** — port 8080 / 80xx / 8888 labeled TLS when the record is TLS; plaintext HTTP on those ports stays HTTP.
- **Payload NetFlow / syslog** — classify from the datagram on custom collector ports (e.g. UDP 515).
- **Richer mDNS** — service types, cache-flush / QU, additional A/AAAA in app decode.

### PCAP over IP (optional)

- Connect to a **remote PCAP stream** you configure (same idea as Wireshark `TCP@host:57012`).
- **Local network permission** is requested only when you use this feature — the app does **not** sniff the iPhone's Wi‑Fi or cellular interface.
- Options → PCAP over IP; invalid host/port errors stay in the connect sheet.

### Fixed

- Live PCAP-over-IP no longer re-analyzes the whole file every second or re-packs the Circle ring.
- Remote-connect failures no longer look like a Files import error.
- Options → PCAP over IP waits for Options to dismiss before presenting the connect sheet.

---

## Still true

| Note | Detail |
|------|--------|
| No live capture on iOS | Analyze PCAP/PCAPNG (and related formats) you open or stream from a **source you choose** |
| Not Wireshark | Native, limited decode — see [Limitations](./LIMITATIONS.md) |
| Privacy | **Data Not Collected** — analysis on device; see [Privacy](../PRIVACY.md) |

---

## App Store submission

Paste-ready **What's New**, review notes, screenshot list, and checklist: **[APP-STORE-1.2.4.md](./APP-STORE-1.2.4.md)**.

---

*PacketCircle — conversation-first triage on iPhone.*

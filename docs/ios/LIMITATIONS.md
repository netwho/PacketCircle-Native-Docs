# PacketCircle iOS: Limitations

<p align="center">
  <img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

> **Disclaimer:** the iOS release is currently under review by the Apple App Store and should be available soon.

PacketCircle iOS favors responsiveness on phone. There is **no hard-coded maximum PCAP file size**, but memory, CPU, and a few feature caps set real expectations. This page is meant to set those expectations clearly.

![TCP health (native estimate)](../assets/ios-session-health.jpg)

## What to expect (quick guidance)

| Situation | Comfortable on iPhone | Possible but tight |
|-----------|------------------------|--------------------|
| **PCAP / PCAPNG size** | ~**50–200 MB** | Larger files may feel slow or get memory-killed on older devices |
| **Packet count** | Hundreds of thousands | Millions — analysis still streams, but UI work (decode/follow) gets heavy |
| **Unique conversations (pairs)** | Hundreds → low thousands | Very large pair sets grow RAM (aggregates are kept in memory) |
| **Circle Top N** | **10 / 25 / 50** (UI) | Only the top N pairs are drawn (Hosts or Services layout); **Talkers lists all** analyzed pairs |
| **Hosts on the ring** | Up to **64** stable slots | Extra hosts may not get a permanent ring slot (Hosts mode) |
| **Services layout** | Same Top‑N pairs on the ring | Bipartite host→service edges only — denser fan-in can still look busy; use legend filters |

**Rule of thumb:** Circle / Gauges / Talkers scale with **how many unique pairs** you have, not only with file size. A 500 MB capture with few talkers can be fine; a smaller file with huge fan-out of unique IPs can still pressure memory.

## Open / analyze (Circle, Gauges, Talkers)

Opening a capture **streams** frames from disk. PacketCircle keeps **aggregates only** (pairs, ports, protocol counts, TCP quality estimates) — not a full in-memory copy of every payload.

| Constraint | Behavior |
|---|---|
| File size | Soft limit (I/O + CPU); no `maxBytes` check |
| Packet count | Soft limit; full sequential pass |
| Memory | Grows with **unique conversations** and **TCP flows** |
| Payloads during analyze | Discarded after classification |

**Stress case:** millions of unique IP pairs (or TCP 5-tuples) can exhaust RAM even when streaming I/O is fine.

## Feature caps (hard numbers)

| Feature | Limit | Notes |
|---|---|---|
| **Decode list** | **500–10 000** frames (Options; default **5 000**) | Rest of the file is ignored for that Decode view |
| **Session → Application decode** | Paginated (**20** rows, Show more); load capped (~**200** frames) | Use pair/port scope |
| **TCP Exchange** | Paginated (**20** rows, Show more) | Same idea — keep Session usable |
| **Follow TCP Stream budget** | **64–2048 KB** (Options slider; default **256 KB**) | Caps reassembled payload + scan; UI shows truncation |
| **Follow absolute ceiling** | Up to ~**2 MB** payload at max budget | Not unlimited |
| **Circle Top N** | **10 / 25 / 50** | Filter-then-Top‑N |
| **Ring capacity** | **64** hosts | Layout / demo stability |
| **Gauge history** | **60** rate samples | Rolling window |

## Demo Mode

Demo Mode loads **every frame + payload** into memory and replays over ~**60 seconds**, then loops.

- Prefer a **small** demo / sample (tens of KB → a few MB)  
- Do **not** treat Demo Mode as a path for huge production captures — that spikes RAM and may kill the app  

Use **Open Capture** for real files; use Demo for the built-in story.

## Replay

Tapping the status bar to **Replay** re-walks timing from the loaded file. Cost rises with file length and how much UI work you do during playback. Prefer moderate captures for smooth replay.

## Data scope & heuristics

- **Conversation vs Sockets** focus changes whether TCP health is IP-pair aggregated or port-specific  
- Client/server without SYN uses a well-known-port heuristic  
- TCP health is a **native estimate** — not Wireshark `tcp.analysis`  
- MAC analysis mode does not use TCP quality coloring (protocol coloring only)

## Formats & capture reality

| Format | Support |
|---|---|
| Classic **PCAP** | Yes |
| **PCAPNG** | Yes |
| gzip (`.pcap.gz`, `.pcapng.gz`) | Yes |
| Sniffer Pro `.cap` / `.caz` | Yes (Ethernet NetXRay; unusual WAN/ATM files may need a Wireshark conversion) |
| Other wiretap-only formats | Not on iOS App Store builds |

- **No live capture** on device (offline open + Demo only)  
- Truncated trailing frames (file still growing when copied) are tolerated; analysis may be **partial**

## Practical recommendations

1. Prefer **filtered** captures (by host/port/time) when you care about Decode or Follow TCP Stream.  
2. Keep iPhone opens in the **~50–200 MB** comfort zone when possible.  
3. If the circle looks crowded, lower **Top N**, switch **Hosts ↔ Services**, or filter protocols/quality grades.  
4. Raise **Follow TCP Stream budget** only when you need more payload — higher values cost CPU/RAM.  
5. Use **Sockets** focus when several services share one IP pair and only one may be slow.  
6. For huge captures, analyze the overview on phone, then finish deep decode on a laptop / Wireshark.

## Tips

1. Prefer smaller / filtered captures for deep decode and follow  
2. Raise the Follow budget only when needed  
3. Use **Sockets** focus to isolate a slow service/port

# PacketCircle iOS: Detailed Usage

<p align="center">
  <img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

> For the current build walkthrough with Simulator screenshots, see the **[User Manual](USER-MANUAL.md)**.  
> For what changed in each version (current **1.3.8**), see the **[Release history](../RELEASES.md)**.

PacketCircle is built for **conversation-first** packet analysis: see who talks to whom, which services matter, where TCP health degrades, then drill into decode and payload when needed — all offline from PCAP / PCAPNG.

This guide explains the **representations**, **controls**, and **analyst workflows** behind each tab.

## Screenshots

| Hosts circle | Services map | Gauges |
|:---:|:---:|:---:|
| ![Hosts](../assets/ios-circle.jpg) | ![Services](../assets/ios-services.jpg) | ![Gauges](../assets/ios-gauges.jpg) |

| Session overview | TCP health | TCP exchange |
|:---:|:---:|:---:|
| ![Session](../assets/ios-session-overview.jpg) | ![TCP health](../assets/ios-session-health.jpg) | ![TCP exchange](../assets/ios-tcp-exchange.jpg) |

| Application decode | Follow TCP Stream | Decode / Options |
|:---:|:---:|:---:|
| ![App decode](../assets/ios-app-decode.jpg) | ![Follow](../assets/ios-follow-tcp-stream.jpg) | ![Decode](../assets/ios-decode.jpg) |

---

## Analyst mental model

Think in layers:

1. **Topology** — Circle (Hosts or Services) answers *structure*: who connects, which apps dominate.
2. **Volume & health** — Gauges (including degraded conversations + quality timeline) + edge coloring answer *intensity* and *pain*.
3. **Ranking** — Talkers answers *priority*: all analyzed pairs by packets or bytes (Circle still uses Top‑N for drawing).
4. **Session** — Session Details answers *why this pair looks odd* (RTT, window, retrans, RST, TLS/ICMP cues).
5. **Evidence** — Decode / Follow TCP Stream answers *what was said* (frames, hex, reassembled payload).

Open Capture always analyzes the **full** file into aggregates. Demo / Replay adds time so you can watch the story unfold.

---

## Circle — two representations

Toggle **Hosts** / **Services** (top-right chip on the Circle tab). Both use the same Top‑N conversations and legend filters; they answer different questions.

### Hosts mode (classic ring)

| Visual | Meaning for the analyst |
|--------|-------------------------|
| **Nodes on the ring** | Unique endpoints (IP or MAC, depending on analysis mode) |
| **Chord / edge** | A conversation pair (undirected host↔host) |
| **Edge thickness** | Relative volume (packets or bytes — Weight in the view menu) |
| **Edge color · Protocol** | Dominant / display protocol for that pair |
| **Edge color · Quality** | TCP session grade (Excellent → Poor); MAC mode stays protocol-colored |
| **Legend chips** | Solo/multi filter by protocol (and related category/grade filters) |
| **Top N** | Only the heaviest pairs are drawn — reduces hairballs |

**Use when:** you care about *host communities*, east–west chatter, or a single host fan-out to many peers.

**Gestures:** pinch to zoom · drag to pan · tap an edge for the connection card · tap a node to highlight its neighborhood.

### Services mode (bipartite host → service)

| Visual | Meaning for the analyst |
|--------|-------------------------|
| **Left arc (hosts)** | Endpoints that originate / participate in service traffic |
| **Right column (services)** | Application / service labels (HTTP, SSH, FTP, Telnet, SMB, DNS, …) |
| **Edges** | Only **host → service** links (no host↔host chords) |
| **Service color** | Stable protocol palette (matches legend) |
| **Fan-in / fan-out** | Many hosts → one service = popular app; one host → many services = multi-role station |

**Use when:** you care about *what was spoken*, service exposure, or “which hosts touched cleartext Telnet / FTP?” without drowning in peer-to-peer chords.

> Tip: solo a legend chip (e.g. **SMB**) in either mode to reduce noise before opening Session Details.

---

## Gauges — rates, degraded quality, timeline

Gauges summarize the capture (and live Demo / Replay ticks) as:

- **Packet / byte / error-style rate** strips over a short rolling history  
- **Top talkers** by bytes and by packets  
- **Top protocols** by share  
- **Degraded Quality Conversations** — **Fair** and **Poor** only, worst first; tap a row → **Session details**  
- **Quality timeline** — graded bars over capture time (idle gaps compressed); **tap** → choose a ± time slice → opens **Talkers** filtered to that window  

In-app **?** help sheets explain both quality panels and how Lenient / Balanced / Strict Options presets map scores to grades.

**Use when:** you need a dashboard answer — “is this capture bursty?”, “which pair owns the bytes?”, “when did quality go Fair/Poor?” — before committing to a deep dive.

---

## Talkers — ranked conversations

Talkers is the **ordered work queue** for analysts. It lists **all** analyzed conversations (not capped like the Circle ring).

| Control | Behavior |
|---------|----------|
| **#1 … #N** | Rank by packets or bytes (matching Weight) |
| **Top 10 / 25 / 50** | Applies to **Circle** drawing only — Talkers still shows the full list |
| **IP filter** | Narrow the list without re-analyzing the file |
| **Time slice** | Optional window from the Gauges quality timeline |
| **Checkbox** | Multi-select conversations |
| **Select all / Clear** | Bulk selection helpers |

Each row uses the same **bi-directional** conversation summary as Circle / Session. Optional **Show host names** shortens DNS / mDNS / NetBIOS labels.

### When anything is checked

A bottom action bar appears:

| Action | Analyst value |
|--------|----------------|
| **Copy Filter** | Wireshark display filter — OR of selected host pairs (paste into Wireshark / other tools) |
| **Show in Decode** | Opens Decode scoped to the selection (multi-pair OR scope) |
| **Save PCAP** | Exports a new PCAP containing only frames that match the selected conversations |

Tap the row (not the checkbox) to open **Session Details**. Context menus still offer single-pair Copy Filter / Show in Decode.

**Use when:** you have already spotted candidates on the Circle and want to batch-export or batch-decode the interesting subset.

---

## Session Details — per-conversation deep dive

Opened from Circle (edge / card), Talkers, or Gauges (degraded list). Layout adapts to what the pair actually carries.

### Conversation summary

Shared bi-directional header:

- Both hosts (optionally shortened names), packets each direction, protocol chips  
- **Whole conversation** — **no ports** (avoids inventing wrong peer services)  
- **Socket selected** — one port each side, e.g. `TCP/80 HTTP` ⟷ `TCP/4444`  
- Unnamed listeners stay visible as sockets (e.g. backchannel **4444**)

### TCP Session Health

Native estimate (not Wireshark `tcp.analysis`), scored 0–100 then graded Excellent / Good / Fair / Poor using Options thresholds.

Typical signals:

- RTT estimate  
- Window min/max (and zero-window counts)  
- Retransmission guesses  
- RST / SYN / FIN presence  
- Grade color (same palette as Quality edge mode)

**Conversation vs Sockets focus (Options):**

- **Conversation** — one health summary for the IP pair (all TCP ports rolled up)  
- **Sockets** — pick a destination TCP port / service; health is port-specific  

Use **Sockets** when one IP pair multiplexes HTTP + SSH + something sick, and only one service is misbehaving.

### TCP Exchange

Client ↔ server ladder of segments (flags, seq/ack, payload lengths). Paginated (**20** rows, then **Show more**) so phones stay usable.

### Application decode / messages

Protocol-aware previews where PacketCircle can summarize them, for example:

- DNS / HTTP / SMB / mail / FTP / Telnet / Kerberos / LDAP / SSH snippets  
- Infra peeks where present: **IPsec**, **BGP**, **OSPF**, **MPLS**, **VXLAN**, **NetFlow/IPFIX**, **LLDP**, **EAPOL**, **STP BPDU**, **mDNS/LLMNR**  
- **TLS** card when certificates / SNI are visible (TLS 1.3 often SNI-only)  
- **ICMP / ICMPv6 / ARP** message lists when that is the conversation’s layer  

Decode labels prefer readable names (operation / class / answers) over bare opcodes. Rows are paginated; use **Show in Decode** for the full capped frame list.

### Follow TCP Stream

From Session Details (TCP pairs): reassembled payload with:

- Directions: Entire / Client→Server / Server→Client  
- Formats: ASCII / Hex / Hex+ASCII  
- **Client = red · Server = blue**  
- Budget capped in Options (default **256 KB**, range **64–2048 KB**)  

Without a clear SYN handshake, client/server uses a well-known-port heuristic.

---

## Decode — frame list + hex

Decode shows a **capped** frame list (Options: **500–10 000**, default **5 000**) with detail tree and hex pane.

| Scope | How you get there |
|-------|-------------------|
| Whole capture (capped) | Open Decode tab with no conversation scope |
| One pair (+ optional port) | Session Details → Show in Decode |
| Several pairs | Talkers multi-select → Show in Decode |

Free-text / hex search further narrows the list. Prefer scoped Decode on large files — it keeps the list about *this* incident.

---

## Open, Demo, and Replay

| Path | What happens |
|------|----------------|
| **Open Capture** | Full offline analyze → aggregates for Circle / Gauges / Talkers |
| **Demo Mode** | Built-in (or small) story, timed loop — great for teaching the UI |
| **Stop Demo** | Finishes into a normal full analyze of that demo capture |
| **Replay** (status bar) | Re-walks the loaded file at original inter-frame timing |

Status bar shows roughly: `<file> · <packets> · <pairs>` and offers Replay when a file is loaded.

---

## Options (gear)

| Setting | Effect |
|---------|--------|
| **TCP health focus** | Conversation vs Sockets (see Session Details) |
| **Quality thresholds** | Lenient / Balanced / Strict — remaps the same 0–100 score into grade bands (color only) |
| **Show host names** | Prefer shortened DNS / mDNS / LLMNR / NetBIOS names on Circle, Talkers, Session (addresses still used for filters) |
| **Follow TCP Stream budget** | Caps reassembly + scan so huge streams don’t freeze the UI |
| **Decode frame limit** | Caps how many frames Decode materializes |
| **Guided tour** | Circle → Session → Gauges; Finish/Skip leaves a clean, unfiltered Circle |

Also available from the Circle view menu: **IP vs MAC** analysis mode, **Packets vs Bytes** weight, **Top N**, **Protocol vs Quality** edge coloring.

---

## Suggested workflows

### “What is this capture about?”

1. Open → glance **Gauges** (top protocols / talkers, degraded list, quality timeline)  
2. **Circle · Services** — which apps appear, who fans into them  
3. Solo noisy protocols off the legend if needed  

### “Find the sick TCP session”

1. Circle menu → Color **Quality**, **or** open **Gauges → Degraded Quality Conversations**  
2. Spot Poor / Fair edges / rows → open Session Details  
3. If the pair hosts several ports, switch Options to **Sockets** and isolate the bad service  
4. Optionally tap the **quality timeline** → time slice → Talkers for that window  
5. Follow TCP Stream or Show in Decode for evidence  

### “Export just the interesting talkers”

1. Talkers → check the ranked rows you care about (full list; Circle Top‑N is separate)  
2. **Save PCAP** for a shareable subset, and/or **Copy Filter** for Wireshark  

### “Cleartext / legacy service exposure”

1. Circle · **Services**  
2. Focus Telnet / FTP / HTTP (cleartext) via legend  
3. Open host→service edges → Follow Stream for commands and banners  

---

## Related

- [Quickstart](./QUICKSTART.md) — shortest path from open → explore  
- [Limitations](./LIMITATIONS.md) — memory, caps, and comfort-zone file sizes  

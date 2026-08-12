# PacketCircle macOS User Manual

<p align="center">
<img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

*Tech Preview (1.1.0). Screenshots below should be captured against the current build using the built-in Demo Mode capture, in **dark mode**, to match the iOS manual's convention.*

---

## 1. Background and motivation

PacketCircle macOS is the desktop half of PacketCircle Native — a Swift/SwiftUI rewrite of the ideas behind the original open-source [PacketCircle Wireshark plugin](https://github.com/netwho/PacketCircle). It shares `PacketCircleCore` with the iOS app: open a PCAP/PCAPNG (or capture live), see who's talking to whom, spot rough TCP health, and drill into a conversation — all without shipping Wireshark or a GPL dissection stack.

### What PacketCircle macOS is

- A **capture visualizer** built around **conversation topology** (the circle), a sortable **pair table**, **rate/quality gauges**, and a **native packet Decode window**.
- A **desktop-native** app: multiple windows (Decode opens separately), **live capture** from local interfaces, **replay** of a capture at original timing, and one-click **handoff to Wireshark** when you need its full dissector depth.
- A **proprietary** product (see [License](../LICENSE.md)), implemented in Swift.

### What PacketCircle macOS is not

**PacketCircle is not a desktop Wireshark.**

It contains no open-source Wireshark / libwireshark dissection. Decode is native and best-effort: solid IP/TCP/UDP detail and some application-layer recognition, not dissector-for-dissector parity with Wireshark. When you need that depth, use **Open in Wireshark** to hand off the capture with the current display filter already copied.

For caveats and capacity notes, see [Limitations](LIMITATIONS.md).

---

## 2. Getting started

| Step | Action |
|------|--------|
| 1 | Open `PacketCircle.app` (Tech Preview DMG — see [Quickstart](QUICKSTART.md) for install). |
| 2 | **File → Open Capture…** (`⌘O`) to load a PCAP/PCAPNG, or **File → Demo Mode** (`⌥⌘D`) if you just want to look around. |
| 3 | For live traffic, **File → Interfaces…** (`⇧⌘I`) instead. |
| 4 | Explore **Circle → Table → Gauges** from the view-options bar; open **Decode** (`⇧⌘D`) when you need packet-level detail. |

**Screenshot 01** — empty main window before any capture is loaded, toolbar visible.

---

## 3. Main window layout

The window is a single split view: a **sidebar** (pair list + protocol legend) on the left, and the active **display mode** (Circle / Table / Gauges) on the right, with a two-row toolbar above.

**Screenshot 02** — full window, Circle view populated from Demo Mode, sidebar and legend visible.

### 3.1 Primary toolbar

| Control | Shortcut | Role |
|---|---|---|
| Open | `⌘O` | Open a PCAP/PCAPNG file |
| Interfaces | `⇧⌘I` | Choose a local interface and start a live capture |
| Transport controls | — | Start / pause / resume / stop the current live capture or replay |
| Save | `⌘S` | Save Capture As… |
| Save to Downloads | `⌥⌘S` | Fallback save if the save panel misbehaves |
| Replay | `⌥⌘R` | Replay the open file at its original inter-frame timing |
| **Reset** | — | Clears selection, protocol filter, and manual direction overrides. Icon fills accent-orange when there's something to clear, greyed out and disabled otherwise. |
| Decode | `⇧⌘D` | Open the Decode window (unfiltered) |
| Wireshark | — | Open the capture in Wireshark with the current display filter copied |
| Close | `⇧⌘W` | Close Capture — clear the session |

### 3.2 View options bar

| Control | Options | Role |
|---|---|---|
| Top N | 10 / 25 / 50 | How many conversations the Circle shows |
| Weight by | Pkts / Bytes | What drives line thickness and Top-N ranking |
| Display mode | Circle / Table / Gauges | Switch the main canvas |
| Address mode | IP / MAC | Layer 3 vs layer 2 pairing |
| Edge color | Proto / Quality | Color arcs by protocol category, or by native TCP session-quality grade |
| Line weight | on/off | Toggle traffic-volume line thickness |

### 3.3 Sidebar

The pair list mirrors what's on the circle (Top-N, filtered by the protocol legend). Hovering or selecting a row highlights the matching arc on the circle and vice versa; the header shows a live count. Below the list, the **protocol legend** lets you enable/disable protocols — disabled protocols disappear from both the list and the circle.

---

## 4. Circle view

Hosts sit on a ring; each conversation is an arc, colored and weighted per the view options above. One-way traffic is dotted with an arrowhead; two-way traffic is a solid line (dual TCP+UDP pairs alternate dashes).

**Gestures**

| Gesture | Effect |
|---|---|
| Pinch / scroll | Zoom |
| Drag | Pan |
| Click an arc | Select the conversation — opens the [connection popup](#5-connection-details-popup) |
| Click a node | Select that host's busiest conversation |
| Right-click an arc | Context menu (below) |
| Right-click a node | Context menu (below) |

**Floating controls** (top-right of the canvas)

| Control | Role |
|---|---|
| Fit to screen | Reset zoom/pan to default |
| Rearrange | Re-lay-out node angles for readability; angles then stay stable as a live graph grows |

**Screenshot 03** — Circle view with one conversation selected (thicker arc + white glow, matching sidebar row highlighted orange).

### 4.1 Reset control

**Screenshot 04** — toolbar Reset button in both states: inactive/grey (nothing to clear) and active/orange (a selection or narrowed protocol filter is in effect).

### 4.2 Right-click on an arc (conversation)

Restart capture · this conversation · Restart capture · *src* · Restart capture · *dst* · Follow TCP Stream · **Open in Decode…** · Copy Wireshark Filter · Copy BPF Filter · Select

**Screenshot 05** — this context menu open on an arc.

### 4.3 Right-click on a node (host)

Restart capture · *host* · Copy BPF · host *host* · Select station

**Screenshot 06** — this context menu open on a node.

---

## 5. Connection details popup

Selecting a conversation (click an arc, a sidebar row, or a node) opens a floating popup on top of the circle with per-port detail for that pair.

**Screenshot 07** — the popup open, showing direction and per-port breakdown.

| Control | Role |
|---|---|
| Direction toggle | Cycle ↔ / → / ← for that pair (affects arrow/dash rendering and traffic totals) |
| Copy filter | Copy a Wireshark display filter, optionally scoped to one port |
| Open in Wireshark… | Hand the capture off to Wireshark with the filter copied |
| Show in Decode | Open the Decode window pre-filtered to this pair (and port, if picked) |
| Follow TCP Stream | Reassemble one TCP flow's payload |
| Close (×) | Dismiss the popup — also clears the selection, so the sidebar row and circle arc stop being highlighted |

---

## 6. Table view

A sortable table of the same Top-N pairs: Source, Destination, Packets, Bytes, and a protocol swatch + label.

**Screenshot 08** — Table view with a few rows visible.

---

## 7. Gauges view

A dashboard of live-updating rate gauges and distribution charts:

- Arc gauges for **packets/s**, **bytes/s**, and **errors/s**, each with a short sparkline history.
- **Top talkers** bar chart (by bytes).
- **Top protocols** bar chart (by packets).

**Screenshot 09** — Gauges view populated from a running or replayed capture.

---

## 8. Decode window

Decode opens in its **own window** (`⇧⌘D`, or the toolbar/menu Decode button) so you can keep the Circle in view while reading packets. It shows a packet list, protocol tree, and hex/ASCII pane, driven by native decode (Ethernet → IP → TCP/UDP → limited application layers).

Open it two ways:

- **Unfiltered** — the Decode toolbar button or `⇧⌘D`, showing the whole capture.
- **Filtered to a pair** — **Open in Decode…** from a context menu, or **Show in Decode** from the connection popup; the window opens scoped to that conversation (and port, if one was picked).

**Screenshot 10** — Decode window filtered to one conversation, packet list + tree + hex all visible.

Font, size, and per-protocol colors for this window are configured in [Settings](#12-settings).

---

## 9. Follow TCP Stream

Reassembles one TCP flow's payload bytes, from a context menu or the connection popup.

| Control | Options |
|---|---|
| Direction | Entire conversation · Client → Server · Server → Client |
| Format | ASCII · Hex · Hex + ASCII |
| Copy | Clipboard |

**Screenshot 11** — Follow TCP Stream window open on a cleartext (e.g. HTTP or FTP) flow.

---

## 10. Live capture

**File → Interfaces…** (`⇧⌘I`) opens a sheet listing local interfaces with live traffic and packets/s columns, a BPF filter field (with a known-hosts helper), and **Promiscuous** / **Hide idle** toggles.

**Screenshot 12** — Interfaces sheet, at least one interface showing live traffic.

Once running, the toolbar shows a **live badge** and a compact stats bar (packets, bytes, duration, rate) over the circle.

**Screenshot 13** — a live capture in progress, live badge and stats bar visible.

| Action | Shortcut |
|---|---|
| Pause / Resume | `⇧⌘P` |
| Stop | `⌘.` |

The capture engine tries `dumpcap`, then libpcap, then prompts for admin `tcpdump` if permission is denied — the first live capture on a fresh machine may trigger a macOS permission prompt.

Right-click actions on the circle (**Restart capture · this conversation** / **· host**) restart the live capture with a BPF filter scoped to just that traffic — handy for narrowing in on one conversation without losing the live view.

---

## 11. Replay and Demo Mode

- **Demo Mode** (`⌥⌘D`) loads a bundled sample capture — the fastest way to see the app populated without your own PCAP.
- **Replay** (`⌥⌘R`) replays an already-open file at its original inter-frame timing, so arcs and gauges animate the way they would live. A progress indicator tracks playback; **Stop** (`⌘.`) ends it and returns to the static full-file view.

---

## 12. Settings

`⌘,` opens Settings, with two tabs:

| Tab | Contents |
|---|---|
| **Decode** | Monospace font picker (only fixed-pitch fonts are listed) and size slider, with a live preview |
| **Colors** | Per-protocol color overrides for the Decode packet list, protocol tree, and hex dump, with **Reset All** |

**Screenshot 14** — Settings → Decode tab.
**Screenshot 15** — Settings → Colors tab.

---

## 13. Wireshark integration

The **Wireshark** toolbar button and the popup's **Open in Wireshark…** both hand the current capture off to a locally installed Wireshark, with the equivalent display filter already copied to the clipboard so you can paste it straight into Wireshark's filter bar. This control is disabled if Wireshark isn't installed.

---

## 14. Keyboard shortcuts (reference)

| Shortcut | Action |
|---|---|
| `⌘O` | Open Capture… |
| `⌘S` | Save Capture As… |
| `⌥⌘S` | Save Capture to Downloads |
| `⇧⌘I` | Interfaces… |
| `⌥⌘D` | Demo Mode |
| `⌥⌘R` | Replay · Original Speed |
| `⇧⌘D` | Decode |
| `⇧⌘P` | Pause / Resume (live capture only) |
| `⌘.` | Stop (live capture or replay) |
| `⇧⌘W` | Close Capture |
| `⌘,` | Settings |
| `⌘Q` | Quit |

---

## 15. Workflows

### A. First look at a capture

1. `⌘O` to open a file (or `⌥⌘D` for Demo Mode).
2. On Circle, skim arcs; switch **Edge color** to Quality to spot Fair/Poor sessions.
3. Click an arc for the connection popup, or open **Table** for a sortable list.

### B. Diagnose a bad TCP session

1. Edge color → **Quality**; look for orange/red arcs.
2. Click the arc → connection popup → check direction and per-port detail.
3. **Follow TCP Stream** for payload, or **Show in Decode** for frame-by-frame detail.
4. **Reset** clears the selection and filter once you're done, ready for the next pair.

### C. Live triage on the wire

1. `⇧⌘I` → pick an interface, optionally set a BPF filter → Start Capturing.
2. Watch Circle/Gauges update live; right-click a conversation → **Restart capture · this conversation** to narrow the live capture to just that traffic.
3. `⇧⌘P` to pause and inspect, `⌘.` to stop and analyze the full trace.

### D. Handoff to Wireshark

1. Select a conversation (or none, for the whole capture).
2. **Wireshark** toolbar button, or the popup's **Open in Wireshark…** — the display filter is copied automatically.

---

## 16. Related documentation

| Doc | Topic |
|---|---|
| [Quickstart](QUICKSTART.md) | Install and first steps |
| [Detailed usage](DETAILED-USAGE.md) | Analyst workflows and alternate screenshot set |
| [Limitations](LIMITATIONS.md) | Capacity, decode depth, what's out of scope |
| [iOS User Manual](../ios/USER-MANUAL.md) | The phone/tablet counterpart |
| [License](../LICENSE.md) | Proprietary terms |
| [Privacy](../PRIVACY.md) | No data collection |

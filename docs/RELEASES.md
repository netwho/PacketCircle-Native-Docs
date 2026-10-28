# PacketCircle Native — Release History

Condensed from the project changelog. Platform pages state which version they describe.

## 1.3.2 — 2026-10-16 (Tech Preview)

Appearance, a connection card that follows the window, and the same analysis footer on Linux and Windows. Marketing version **1.3.2**.

### Added
- **Appearance** — Settings (Mac and Linux/Windows) and Options (iPhone) choose Dark, Light, or the desktop. The circle and the infrastructure map stay black. Light text, icons, lists and charts are darkened so they stay readable.
- **Expert search in Help** — the Circle page explains typing `expert` and a group or message.

### Changed
- **Connection card (Mac)** — a conversation with TCP quality, more than one socket, or Expert findings uses the full height of the circle, with the same margin at the top, right and bottom, and grows or shrinks with the window.
- **Linux / Windows footer** — while a capture is read, the frame counter is on the bottom bar and Stop is at the right, as on the Mac.

---

## 1.3.1 — 2026-10-10 (Tech Preview)

Expert Alerts page, gauge navigation, and save-time sanitizing on the Swift apps. Marketing version **1.3.1** (macOS build **107**, iOS build **23**).

### Added
- **Expert Alerts page (Mac)** — Link, Network, Transport, Application, and All tiles. Totals include notes, so a TTL = 1 finding counts on L3. The list has a band-colour bar; the side panel explains the selected finding.
- **iOS Expert tiles** — the same counters in a horizontal row, including notes.
- **Gauges** — Hosts, Protocols, and TCP health are clickable and apply the Circle’s host, protocol, or grade filter. Problem conversations sit beside the quality timeline.
- **Conversation header** — endpoint blocks (name, address, MAC, port) and stat chips. Application messages draw as a two-lifeline sequence; identical neighbours collapse to ×N.
- **Copy capture filter** — BPF for the pair, narrowed to `tcp port` or `udp port` when a socket is selected.
- **Protocol legend** — disclosure triangle under the pairs list, remembered in UserDefaults.
- **Sanitize on save** — Save, Save Session, and iOS export ask first. The choice lasts for the session.
- **Report logo** — PNG/JPEG up to 2 MB, stored in Application Support, drawn top-left on the cover (max 40 mm) with the PacketCircle mark as fallback.
- **Toolbar** — Info stays enabled (Capture details, or About when nothing is open). Help is at the right. Close capture is a circled ×, disabled when nothing is open. Large-file analysis shows conversations, apps, and frames.

### Notes
- Circle Expert chips still count warnings and errors only. The page tiles are the ones that include notes.
- Hovering a pairs row or a chord still highlights that chord, and now dims the others.
- **Names** sits with IP and MAC. It turns host names on and off. Settings no longer has that switch.
- Giant / jumbo warnings count bytes stored in the capture. A PCAP original length left by snaplen or Linux GRO is not a jumbo frame, so a 1514-byte frame (CRC not stored) stays under the 1518 threshold. Decode shows that claimed original length when it is larger than the stored frame.
- The Linux circle draws one-way conversations as a dotted arrow and multi-protocol conversations as coloured dashes. The arrow in the pairs list chooses the direction. Expert Alerts and conversation quality sit on the circle; the protocol legend uses colour swatches. The pairs pane starts wide enough for two columns of those swatches. Right-click an Expert tile for the alert list (select the conversation, hide that finding for this conversation, or suppress the alert type). Right-click a quality grade to list those conversations and select one. The footer shows packets, bytes, the file name, and how long the analysis took, including Full or Light. The Mac footer shows analysis progress and Stop while a capture is read, then how long it took and whether that was Full or Light (`analyzed (Full) 5.5s`).

---

## 1.2.6 — 2026-09-27 (Tech Preview)

Session Expert scoping and packaging fix. Marketing version **1.2.6** on **macOS and iOS** (App Store Connect needed a new version after 1.2.5). The first App Store build with the 1.2.5 items below ships as 1.2.6.

### Added
- **Compare Conversations (Mac) shows which side is worse** — timing, grade, score, RST, retransmits, zero window, MSS and fragmentation colour the worse value amber → red by gap size and the better value green, with a `worse · Δ 212 ms · ×3.4` label. TLS alerts, certificate issues and TCP experts on one side only count as worse. Small gaps stay neutral.
- Expected differences (capture, endpoints, MACs, IPs, IP ID) are dimmed and hidden from **Differences only** unless **Expected differences** is ticked. A legend and an "A worse / B worse" count sit above the table.
- **Compare Conversations on wide iPhones (preview)** — Session shows **Compare with another conversation…** when the screen is at least 600 pt wide (unfolded foldable, Pro Max in landscape). Same two-column table and colours as the Mac. Debug builds have **Options → Developer → Force wide Compare** for testing on any simulator.

### Fixed
- **Session / Connection Expert follows the socket** — viewing one socket (e.g. HTTP/80) no longer lists FTP or other services' findings from the same IP pair; host-pair L3 / DLC findings still show. An **Other sockets** list at the bottom switches sockets and marks the ones with alerts.
- **Same finding on two sockets** (e.g. HTTP 404 on :80 and :8080) is kept per socket instead of merged into one Expert Alerts row.
- **macOS packaging** — Developer ID signing no longer fails on newer SwiftPM resource bundles (`Contents/Info.plist` layout), so the notarized DMG is produced again.

### Notes
- Tech Preview: native estimates and limited decode — not Wireshark. Analysis stays on device.
- iOS TestFlight / App Store: marketing **1.2.6**, `CFBundleVersion` **22** (bump build only for further archives before submit).

---

## 1.2.5 — 2026-09-24 (Tech Preview)

Shared Core polish plus iOS Infrastructure Map. Marketing version **1.2.5** on **macOS and iOS**.

### Added
- **NetBIOS-NS / NetBIOS-DGM / NetBIOS-SSN** — distinct port labels (UDP/TCP 137–139).
- **NetBIOS Name Service decode** — queries/responses, NB records, NBSTAT name tables; host-name harvest.
- **NetBIOS Datagram + Browser** — source/dest names, `\MAILSLOT\BROWSE`, HostAnnouncement and related browse opcodes in Decode and Session Application decode.
- **iOS Infrastructure Map** — third Circle layout (Hosts → Services → Map) from STP / CDP / LLDP / routing; tap a node to open Session. Shared layout engine with Mac Map.
- **Show Infrastructure Map** — Settings / Options → Circle; turn off to hide Map from the layout cycle (on by default).

### Fixed
- Session Application decode no longer shows only NetBIOS-DGM header flags/IDs when mailslot/Browser payload is present.
- **NetBIOS Browser election parse** — no longer traps on large uptime values (UInt32 overflow).
- **Kerberos Expert** — only flags traffic on Kerberos ports (88 / 464), not every TCP stream.
- **Jumbo / giant frame Expert** — based on wire `orig_len`, not reassembly artifacts.
- **Reset filters** — restores IP view when Circle was stuck in DLC / MAC mode.
- **Show host names** — defaults on for new installs (existing prefs unchanged).

### Notes
- Tech Preview: native estimates and limited decode — not Wireshark. Analysis stays on device.
- iOS: 1.2.5 build 21 was not released; these changes ship to the App Store as 1.2.6.

---

## 1.2.4 — 2026-09-01 (Tech Preview)

Capture/decode depth and iOS live-stream polish. Marketing version **1.2.4**.

### Added
- **IGMP / MLD decode** — group addresses, v1/v2/v3 queries and reports (source lists, INCLUDE/EXCLUDE), well-known multicast labels (SSDP, mDNS, all-systems).
- **TLS on HTTP alt ports** — HTTPS on 8080 / 80xx / 8888 is labeled TLS when the payload is a TLS record; plaintext HTTP on those ports stays HTTP.
- **PCAP over IP (client)** — connect to a remote classic-PCAP stream (`TCP@host:57012`), same idea as Wireshark `-k -i TCP@host:port`.
- **Payload NetFlow / syslog** — custom collector ports (e.g. UDP 515) classify from the datagram, not only IANA 2055/514.
- **Richer mDNS** — Bonjour service types, cache-flush / QU, additional A/AAAA.

### Fixed
- Live PCAP-over-IP no longer re-analyzes the whole file every second or re-packs the Circle ring.
- iOS remote-connect failures no longer look like a Files import error; invalid host/port is shown in the sheet.
- Options → PCAP over IP waits for Options to dismiss before presenting the connect sheet.

### Notes
- Tech Preview: native estimates and limited decode — not Wireshark. Analysis stays on device.
- iOS TestFlight: marketing **1.2.4**, `CFBundleVersion` **20**.

---

## 1.2.3 — 2026-08-28 (Tech Preview)

Expert messages in reports, tunable expert thresholds, and ARP / Circle Expert polish. Marketing version **1.2.3**.

### Added
- **Report Expert messages** — Detailed and Field guide PDFs list capture-wide expert types (not only TCP conversation notes): id, trigger, typical use, collapsed count, sample nodes/pairs. Informational types note when an observation may not mean a connectivity or performance outage.
- **Settings → Expert sliders** (iOS Options too) — ARP storm warn/error (who-has per second), unanswered ARP retry count, giant-frame size, endpoint retx %, zero-window count. ARP gates re-apply without a rescan; giant-frame size applies on the next analysis.
- **ARP experts** — request storms and unanswered who-has (retried with no reply in the file).
- **Experts Circle mode** — toolbar after Top 10/25/50 shows conversations with warn/error Expert Alerts (cap 250), ignoring Top N. Solo an Expert Alerts band first to restrict the layer.

### Notes
- Tech Preview: native estimates and limited decode — not Wireshark. Analysis stays on device.
- 1.2.2 remains the Decode display-filter drop.

---

## 1.2.2 — 2026-08-28 (Tech Preview)

Decode display filter plus marketing version **1.2.2**.

### Added
- **Decode display filter** — a second field next to search, using Wireshark display-filter syntax (`tcp`, `tcp.port == 80`, `ip.addr == …`, `http and tcp.flags.syn`, `contains`, `&&` / `||` / `not`). Autocomplete offers only protocols and fields PacketCircle actually decodes; unknown names (e.g. `ospf`, `wlan`, `tcp.stream`) are errors and do not silently match. Invalid filters leave the packet list unfiltered.
- **`tcp.analysis.*`** on Decode for native per-frame experts we already raise: retransmission, spurious retransmission, ACKed unseen (`ack_lost_segment`), zero window, duplicate ACK. Not Wireshark’s analysis engine; out-of-order / `tcp.stream` remain unavailable.

### Notes
- Tech Preview: native estimates and limited decode — not Wireshark. Analysis stays on device.
- Search (text / hex) is unchanged and separate from the display filter.

---

## 1.2.1 — 2026-08-21 (Tech Preview)

macOS triage and Expert polish on top of the 1.2.0 Pro tools shell. Marketing version **1.2.1**.

### Added
- **TCP sequence experts** — **ACKed unseen segment**, **Spurious retransmission**, and **SACK analysis** (invalid block, D-SACK, SACK for unseen data, leftover block). Native SEQ/ACK + SACK-block tracking; they do not change the health score.
- **Suppress alert types** from a row in the Expert Alerts list; re-enable under **Settings → Expert**.
- **Settings → Expert** — TCP retx / zero-window thresholds, encrypted TLS alert toggle, STP/topology in conversation Expert, suppressed-ID list.
- Per-frame **Expert Info** in Decode (tree section + parent-layer badges); opening Decode from alerts auto-expands Expert context.
- Richer **TLS alert** decode/expert: cleartext level + RFC description + Meaning; encrypted/opaque alerts labeled honestly (not fake “Level 42”).
- Broader **App / L3 experts**: HTTP status, TLS, SMTP/POP3/IMAP, FTP, DNS RCODEs, LDAP, Kerberos, SSH; ICMP unreachable / filtered / time-exceeded and related codes; **Duplicate IP address** (conflicting ARP claimants) and **Local routing** (same-subnet traffic hairpinned via the default gateway).
- **iOS 1.2.1**: Expert Alerts bar on Circle (tap to filter, list button / touch-and-hold for the alert sheet), Session **Expert** panel, Decode tree Expert Info badges. Same `PacketCircleCore` findings as Mac. Suppress / Settings → Expert remains Mac-only.
- **iOS Sanitize Capture** — Options → Capture tools, and Capture details (tap file name above the tab bar); native scrub → save PCAPNG via Files.
- **iOS Generate Report** — same places as Sanitize; PDF from native analysis → save via Files. Cover, type, paper, and host-name options match Mac.
- Expert Alerts row → opens the conversation and **pre-selects the socket** that raised the alert when several ports exist.
- **`--window-size` / `PC_WINDOW_SIZE`** (e.g. `1920x1080`) for screenshot-friendly main **content** size (`./build.sh --run --window-size …`).
- Web what’s-new draft: [`docs/WEB-1.2.1-TECH-PREVIEW.md`](docs/WEB-1.2.1-TECH-PREVIEW.md).

### Fixed
- ICMPv6 Neighbor Solicitation is no longer drawn as a one-way dashed chord to solicited-node multicast (`ff02::1:ff…`); the pair uses the NS Target Address (IPv4-mapped targets become dotted IPv4). IPv6 labels use compressed form (`ff02::1:ff9d:2834`).
- Expert Alerts **DLC** runt detection no longer treats normal unpadded Ethernet frames (e.g. 54-byte TCP ACKs) as runts; demo PCAP includes intentional DLC errors (runts + giant).
- Expert Alerts chip context menu is browse-only (**Show … alerts…**); **Suppress** lives only on list rows.

### Notes
- Tech Preview: native estimates and limited decode — not Wireshark. Analysis stays on device.

---

## 1.2.0 — Pro tools shell (2026-08-17)

### Added
- **Settings sidebar** — General, Quality, Expert, Sanitize, Report, Decode, Colors.
- **`PCSettings`** in PacketCircleCore — shared defaults for Sanitize, Report cover page, Expert thresholds.
- **Tools menu** — labeled File / Capture / Tools groups (no hover required for names).
- **Generate Report…** — multi-section PDF with file summary (SHA-256), bar charts, port/TCP analysis, annotation sidebars (detailed/annotated).
- **`PacketSanitizer`** in PacketCircleCore — streaming PCAP/PCAPNG scrub with checksum fixups.
- **Sanitize Capture…** — native `PacketSanitizer` engine (cleartext / all payload / payload + IP/MAC); writes PCAPNG via save panel.
- Toolbar **Report / Sanitize / Decode** labeled cluster.
- **Compare Conversations…** — side-by-side A/B (two edges or second capture): MAC/IP, TCP setup, RTT, certs, findings.
- **Topology Map** — L2/L3 map with STP root-change / TCN signals surfaced in conversation Expert.
- Connection panel **Expert** list — capture App/L3/DLC alerts for the pair merged with TCP quality experts.
- Project skill: `.cursor/skills/packetcircle-native/SKILL.md`.

### Notes
- No Qt, Cairo, GLib, epan, or wtap in shipping builds.
- Wireshark plugins remain separate GPL products; Native reimplements engines in Core.

---

## 1.1.0 — prior tech preview

Circle, Talkers, Services layout, Gauges, Session / TCP health, Follow TCP Stream, native Decode, live capture (Mac), save options, guided tour / demo, quality coloring, and related foundations.

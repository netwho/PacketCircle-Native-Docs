# PacketCircle macOS — Detailed Usage

<p align="center">
<img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

This page covers analyst-oriented workflows in more depth than the [Quickstart](QUICKSTART.md). For a full control-by-control reference, see the [User Manual](USER-MANUAL.md).

## Mental model

Three views over the same Top-N pairs:

- **Circle** — spatial: who's talking to whom, weighted by traffic, colored by protocol or quality. Best for a first look and for spotting outliers.
- **Table** — the same pairs as a sortable list, when you want exact numbers rather than a picture.
- **Gauges** — rates and distributions over time (packets/s, bytes/s, errors/s, top talkers, top protocols) — best when you're watching a live capture or replay.

The **protocol legend** in the sidebar is a filter that applies to all three: disable a protocol there and it disappears from Circle, Table, and the sidebar pair list together.

## Workflow: triage a capture you've never seen

1. Open the file (`⌘O`) or start a live capture (`⇧⌘I`).
2. On **Circle**, switch **Edge color** to **Quality** — Fair/Poor arcs jump out immediately.
3. Click a problem arc → the connection popup gives you direction, per-port breakdown, and one-click paths into **Follow TCP Stream** or **Show in Decode**.
4. Use **Reset** to clear the selection and go back to a clean circle before picking the next one.

## Workflow: narrow a live capture without losing it

1. Start a live capture (`⇧⌘I`), optionally with a starting BPF filter.
2. Once you spot the conversation you care about on Circle, right-click its arc → **Restart capture · this conversation** (or **· *host*** for one endpoint).
3. The live capture restarts scoped to just that traffic — useful when the unfiltered feed is too noisy to watch in real time.
4. `⇧⌘P` to pause and read, `⌘.` to stop and drop into full post-capture analysis.

## Workflow: reconstruct a cleartext exchange

1. Filter the protocol legend down to HTTP, FTP, Telnet, DNS, etc.
2. From the connection popup or the arc's context menu, choose **Follow TCP Stream**.
3. Switch between **ASCII**, **Hex**, and **Hex + ASCII**, and pick a direction (entire conversation, client→server, or server→client) to isolate one side.
4. **Copy** the reassembled text to paste elsewhere.

Expect this to work well for cleartext protocols and to stop quickly at anything encrypted (TLS, SSH payload, etc.) — see [Limitations](LIMITATIONS.md).

## Workflow: hand off to Wireshark mid-analysis

You don't have to choose between PacketCircle and Wireshark up front. A common pattern:

1. Use PacketCircle's Circle/Gauges to find the interesting conversation fast.
2. Select it, then **Open in Wireshark…** — the display filter for that exact conversation is copied automatically.
3. Do the deep-dissector work in Wireshark; come back to PacketCircle for the next pass over the rest of the capture.

## Related documentation

| Doc | Topic |
|---|---|
| [User Manual](USER-MANUAL.md) | Every screen and control |
| [Quickstart](QUICKSTART.md) | Install and first steps |
| [Limitations](LIMITATIONS.md) | What this is (and isn't) |

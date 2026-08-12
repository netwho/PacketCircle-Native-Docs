# PacketCircle macOS — Limitations

<p align="center">
<img src="../assets/logo.png" alt="PacketCircle" width="180" />
</p>

PacketCircle macOS is a **native, proprietary** analysis tool — not a repackaging of Wireshark, and not a claim of parity with it. Read this before treating its output as a substitute for Wireshark in an investigation.

## Decode depth

- No open-source Wireshark / libwireshark dissection is included.
- Native decode covers Ethernet → IP → TCP/UDP solidly, with **best-effort** recognition of common application protocols (HTTP, DNS, SSH banners, FTP/Telnet ASCII, SMB headers, TLS SNI where detectable, and others).
- Uncommon, encrypted, or unusual protocols will typically stop at TCP/UDP with little or no application-layer detail.
- There is no claim of parity with Wireshark's expert infos or `tcp.analysis` fields. Where PacketCircle shows a **TCP session-quality grade** (Excellent/Good/Fair/Poor), that's a native heuristic (RST, retransmit rate, zero-window events, handshake RTT) — useful for triage, not a substitute for Wireshark's analysis.

## When to hand off to Wireshark

Use the **Wireshark** toolbar button (or **Open in Wireshark…** from the connection popup) any time you need:

- Full protocol dissector coverage.
- Expert-info / `tcp.analysis.*` fields.
- Anything beyond triage-level detail.

The current display filter is copied to the clipboard automatically as part of the handoff.

## Live capture

- Live capture depends on local OS capture permissions. The engine tries `dumpcap`, then libpcap, then falls back to prompting for admin `tcpdump` access — expect a macOS permission prompt on first use on a fresh machine.
- Very large captures or very high-rate live interfaces will naturally be slower to render and analyze than a trimmed sample; there's no artificial packet cap, but performance is bounded by your Mac's resources.

## Tech Preview status

This build is an early **Tech Preview**: expect UI and behavior to change between builds, and file issues for anything that looks wrong rather than assuming it's expected.

## Related documentation

| Doc | Topic |
|---|---|
| [User Manual](USER-MANUAL.md) | Every screen and control |
| [Quickstart](QUICKSTART.md) | Install and first steps |
| [Detailed usage](DETAILED-USAGE.md) | Analyst workflows |

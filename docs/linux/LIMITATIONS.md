# PacketCircle for Linux — Limitations

PacketCircle is a **triage and orientation** tool, not a protocol analyzer.

## Analysis

- **No Wireshark dissection.** Decode covers IP, TCP, UDP, ICMP, ARP and recognition of common application protocols (HTTP, TLS, DNS, SMTP, FTP, SSH, Kerberos, LDAP, NetBIOS and more). It is not comparable to Wireshark's coverage.
- **TCP quality is an estimate.** Scores come from native heuristics (retransmits, resets, zero windows, RTT). `tcp.analysis.*` in the Decode filter matches these native findings, not Wireshark's analysis engine.
- **No decryption.** Encrypted payload (TLS, SSH, IPsec) stays opaque.
- **Wi-Fi monitor mode** analysis (RSSI, BSSID coloring) is limited compared with Wireshark.
- Other capture formats: convert with `editcap` first.

## Capacity

There is no hard maximum capture size; memory grows with unique conversations and TCP flows, not file size.

| Item | Limit |
|---|---|
| Comfortable | Hundreds of MB to low GB |
| Tight | Multi-GB files, or millions of unique IP pairs / TCP flows |
| Decode list | Configurable maximum number of frames (default 25 000) |
| Follow TCP stream | 2 MB reassembled payload |
| Circle Top-N | 10 / 25 / 50 |
| Session → PCAP export | Matching frames are held in RAM before writing |
| Live capture | Growing file is re-analyzed about once per second; long sessions get CPU-heavy |

For huge captures choose **Light** analysis depth.

## Platform

- **Live capture** needs libpcap and the one-time `setcap` grant (or `sudo`).
- **GPU required.** The UI renders with wgpu; very old or virtual machines may need `WGPU_BACKEND=gl`.
- **File dialog** needs `xdg-desktop-portal`; otherwise pass the file as an argument or drag it onto the window.
- **Distribution.** Binaries need a glibc as new as the build machine's. Alpine (musl) is not supported by the prebuilt packages.
- **Wireshark integration** requires Wireshark to be installed separately.
- No App Store / Flatpak / Snap package yet.

## Tech Preview

Versions are Tech Previews: behavior, layout and settings may change between releases. Releases are listed in [Release history](../RELEASES.md).

# App Store Connect — PacketCircle iOS 1.2.4

Internal checklist and **copy-paste blocks** for [App Store Connect](https://appstoreconnect.apple.com).

| Field | Value |
|-------|--------|
| **App name** | PacketCircle |
| **Bundle ID** | `com.netwho.PacketCircle` |
| **Marketing version** | **1.2.4** |
| **Build** | **20** (bump `CURRENT_PROJECT_VERSION` in `project.yml` if ASC rejects duplicate) |
| **Team ID** | `8BC35K7B5Y` |
| **Privacy URL** | `https://github.com/netwho/PacketCircle-iOS_macOS-docs/blob/main/docs/PRIVACY.md` |
| **License URL** | `https://github.com/netwho/PacketCircle-iOS_macOS-docs/blob/main/docs/LICENSE.md` |

---

## 1. Build & upload (Xcode)

```bash
cd /Users/walterh/Cursor/PacketCircleNative/iOS/PacketCircleiOS
./generate.sh
open PacketCircleiOS.xcodeproj
```

1. Scheme **PacketCircleiOS** · destination **Any iOS Device (arm64)**
2. Signing → Team **Walter Hofstetter (8BC35K7B5Y)** · Automatic · **Apple Distribution** for archive
3. **Product → Archive** → **Distribute App** → **App Store Connect** → **Upload**
4. Wait for processing under **TestFlight** (5–30 min typical)

---

## 2. Screenshots (iPhone 6.7")

Regenerate dark-mode Simulator shots:

```bash
cd /Users/walterh/Cursor/PacketCircleNative
PC_SIM_DEVICE="iPhone 17" PC_APPEARANCE=dark PC_SHOW_HOST_NAMES=1 \
  ./scripts/capture_manual_screenshots.sh
```

Copy to docs repo for hosting / reference:

```bash
mkdir -p /Users/walterh/Cursor/PacketCircle-iOS_macOS-docs/docs/assets/manual
cp docs/images/manual/*.png /Users/walterh/Cursor/PacketCircle-iOS_macOS-docs/docs/assets/manual/
```

### Recommended App Store order (6.7" display)

Upload **5–6** PNGs to **iPhone 6.7" Display**:

| Order | File | Caption idea (optional in design) |
|-------|------|----------------------------------|
| 1 | `02-demo-circle.png` | See every conversation at a glance |
| 2 | `07-session.png` | TCP health and application decode |
| 3 | `08-session-charts.png` | Quality charts per socket |
| 4 | `11-gauges.png` | Rates and degraded conversations |
| 5 | `10-talkers.png` | Full talkers work queue |
| 6 | `12-decode.png` | Native decode on device |

Native Simulator size is fine; Apple accepts iPhone 17 class for 6.7" slot.

---

## 3. What's New (paste into App Store Connect)

```
• IGMP / MLD decode — multicast groups, queries, and reports
• TLS on common alternate HTTP ports (8080, 8888, …)
• Connect to remote PCAP-over-IP streams (Wireshark-style TCP source)
• Richer mDNS and payload-based NetFlow / syslog classification
• Smoother live PCAP-over-IP updates and clearer connection errors
```

### Promotional text (optional, no review)

```
Offline PCAP triage on iPhone — Circle, Session quality, native decode. Free, no ads, no account.
```

---

## 4. App Review notes (paste into Review Information → Notes)

```
PacketCircle analyzes PCAP/PCAPNG capture files on the device. All processing is local; we do not operate a backend that receives user captures.

Demo Mode replays a bundled synthetic lab capture (PacketCircleDemo.pcap). No login or account is required.

Optional "PCAP over IP" (Options) lets the user connect outbound to a host:port they enter, to receive a remote classic-PCAP stream (similar to Wireshark TCP@host:port). This is NOT live Wi‑Fi or cellular interface capture on the iPhone. Local Network permission is shown only when the user initiates this connection.

The app does not include third-party analytics, advertising, or tracking SDKs. App Privacy is declared as Data Not Collected.

To test: launch app → tap Demo or open a .pcap from Files → Circle → tap a conversation → Session panels.
```

---

## 5. App Privacy (confirm unchanged)

- **Data Not Collected** (primary)
- If prompted for **Local Network**: functionality tied to user-initiated PCAP-over-IP only; not used for tracking

---

## 6. Export compliance

Typically: **No** custom encryption beyond HTTPS/TLS standard APIs, or use standard exemption questionnaire answers for apps that only use Apple's encryption.

---

## 7. Submit checklist

- [ ] Build **1.2.4 (20)** processed in TestFlight
- [ ] Internal TestFlight smoke test (Demo, Session, Decode, PCAP-over-IP if marketed)
- [ ] 5–6 screenshots on **6.7"** slot
- [ ] What's New pasted
- [ ] Review notes pasted
- [ ] Privacy URLs live
- [ ] Version selected → **Add for Review** → **Submit**

After approval: release manually or automatic; update [LIMITATIONS.md](./LIMITATIONS.md) disclaimer; announce 1.2.4 live.

---

## 8. If build number rejected

```yaml
# iOS/PacketCircleiOS/project.yml
CURRENT_PROJECT_VERSION: "21"   # was 20
```

```bash
./generate.sh
# Archive & upload again — keep MARKETING_VERSION at 1.2.4
```

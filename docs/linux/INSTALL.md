# PacketCircle for Linux — Installation

<p align="center"><img src="../assets/linux/L-00-hero.jpg" alt="PacketCircle on Linux" width="720" /></p>

PacketCircle runs on Linux as a native desktop app (egui, Rust-only — no Swift, no Wireshark). Ubuntu 24.04 is the reference platform; any glibc-based distribution with the same or newer glibc works.

**Quick links:** [Quickstart](./QUICKSTART.md) · [User manual](./USER-MANUAL.md) · [Limitations](./LIMITATIONS.md) · [Master document](../PACKETCIRCLE-NATIVE.md)

## 1. Requirements

| | |
|---|---|
| **OS** | Ubuntu 24.04+ (x86_64 or aarch64), Debian 13+, Fedora 40+, Arch, openSUSE Tumbleweed. A build made on Ubuntu 22.04 reaches Debian 12 / RHEL 9 class systems. Alpine (musl) needs its own build. |
| **Display** | A desktop session (Wayland or X11) with a working GPU driver (Vulkan or OpenGL) |
| **Runtime libraries** | `libxkbcommon0 libwayland-client0 libx11-6 libxcursor1 libxi6 libxrandr2 libxcb1 libvulkan1 mesa-vulkan-drivers libegl1 libgl1` |
| **Recommended** | `xdg-desktop-portal xdg-desktop-portal-gtk` for the *Open…* dialog |
| **Live capture** | `libpcap` and a one-time capability grant (see [section 5](#5-allow-live-capture)) |
| **Optional** | Wireshark, for *Send to Wireshark* |

## 2. Choose an install method

The release contains the same signed files in three forms:

| File | Use it when |
|---|---|
| `packetcircle_<version>_<arm64\|amd64>.deb` | You are on Debian / Ubuntu / Mint and want a normal package |
| `packetcircle-<version>-linux-<arch>.run` | You want a self-extracting installer that checks your machine first (any supported distro) |
| `packetcircle-<version>-linux-<arch>.tar.gz` | You prefer an archive; run `./install.sh` inside |
| `packetcircle-ubuntu-<version>.zip` | You want to **build from source** ([section 4](#4-build-from-source)) |

Every file ships with a `.asc` GPG signature and a `SHA256SUMS-<version>-<arch>` file.

## 3. Install

### 3.1 Debian / Ubuntu package

```bash
sudo apt install ./packetcircle_1.3.2_amd64.deb
```

### 3.2 The `.run` installer

```bash
sh packetcircle-1.3.2-linux-x86_64.run --check      # only check this machine (CPU, glibc, libraries, signature)
sh packetcircle-1.3.2-linux-x86_64.run              # interactive: this user or everyone?
sh packetcircle-1.3.2-linux-x86_64.run --user       # ~/.local, no root
sh packetcircle-1.3.2-linux-x86_64.run --system --install-deps   # /usr/local; installs missing packages
sh packetcircle-1.3.2-linux-x86_64.run --capture    # also allow live capture
```

The installer verifies the architecture, glibc version, window/graphics/libpcap libraries, the SHA-256 sums and the GPG signature against the pinned release key. It installs the program, a menu entry, an icon, `packetcircle-capture`, and an uninstaller, and lists everything in `<share>/packetcircle/install-manifest`. Package names are suggested for Debian/Ubuntu/Mint, Fedora/RHEL, Arch and openSUSE.

### 3.3 Verify the signature yourself (optional)

Release signing key fingerprint:

```
861C D064 58D7 CE58 CB42 B5C2 E6C8 0DBE CC1A 298A
```

```bash
gpg --import my-public-key.asc
gpg --verify packetcircle_1.3.2_amd64.deb.asc packetcircle_1.3.2_amd64.deb
sha256sum -c SHA256SUMS-1.3.2-amd64
```

### 3.4 Uninstall

```bash
packetcircle-uninstall            # remove the program
packetcircle-uninstall --purge    # also remove settings
sudo apt remove packetcircle      # if installed from the .deb
```

## 4. Build from source

```bash
unzip packetcircle-ubuntu-1.3.2.zip && cd packetcircle-ubuntu-1.3.2
./scripts/ubuntu/check-deps.sh          # what is missing? (changes nothing)
./scripts/ubuntu/build.sh --yes         # install missing packages + Rust, then build
./dist/packetcircle capture.pcapng      # run it from a desktop session
```

`--yes` installs missing apt packages with `sudo` and Rust with `rustup` (into `~/.cargo`, no sudo).

| Command | What it does |
|---|---|
| `check-deps.sh` | Reports OS, CPU, disk, memory, apt packages, Rust ≥ 1.88, network, display |
| `build.sh` | Checks, then `cargo build --release --locked -p pc-egui` |
| `build.sh --yes --test` | Installs what is missing, builds, runs the tests |
| `build.sh --jobs 2` | Fewer parallel jobs on a small machine |
| `build.sh --check-only` | Dependencies only |

Needs about 8 GB free disk, 3 GB RAM or more (2 GB machines: `--jobs 1` plus swap), Rust ≥ 1.88 (the apt `rustc` is too old; the script uses `rustup`), and internet access during the build (behind a proxy set `HTTPS_PROXY`).

`build.sh` ends by packaging the `.run`, `.tar.gz`, `.deb`, checksums and signatures in `dist/`. Signing needs the release key; use `--no-sign` for a local unsigned build.

## 5. Allow live capture

Opening files needs no special permission. Capturing from a network interface needs it once:

```bash
packetcircle-capture enable        # installed program (also: disable | status)
# or at install time:
sh packetcircle-*.run --capture
# or for a build in dist/:
sudo setcap cap_net_raw,cap_net_admin=eip dist/packetcircle
```

You can also start PacketCircle with `sudo`. **PCAP over IP** needs no permission.

## 6. First launch

Start **PacketCircle** from the application menu, or:

```bash
packetcircle                      # empty start page
packetcircle capture.pcapng       # open a capture directly
```

You can also drag a capture onto the window. Continue with the [Quickstart](./QUICKSTART.md).

## 7. Troubleshooting

| Symptom | Fix |
|---|---|
| No window / "no suitable adapter" | No GPU driver. Install `mesa-vulkan-drivers libegl1`. On a VM without 3D acceleration run `WGPU_BACKEND=gl packetcircle`. Use a real desktop session; X11 forwarding over SSH is slow. |
| *Open…* does nothing | Install `xdg-desktop-portal xdg-desktop-portal-gtk`, pass the file as an argument, or drop it on the window. |
| Out of memory while linking | `./scripts/ubuntu/build.sh --jobs 1` and add swap. |
| Capture list is empty or says permission denied | Run `packetcircle-capture enable` (section 5). |
| Installer says glibc too old | Build from source on the oldest system you want to support. |

Build problems are logged to `build.log` in the unpacked folder; send that file with any report.

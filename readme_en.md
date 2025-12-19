# [WGCF] Connect CF WARP to Add IPv4/IPv6 Networks for Servers

---

## Table of Contents

- [Changelog](#changelog)
- [Script Features](#script-features)
- [Benefits of WARP](#benefits-of-warp)
- [warp Run Script](#warp-run-script)
- [warp-go Run Script](#warp-go-run-script)
- [Cloudflare API](#cloudflare-api)
- [Method to Unlock Netflix with WARP IP](#method-to-unlock-netflix-with-warp-ip)
- [WARP SOCKS5 or Interface Split Tunneling Templates and chatGPT Unlock Methods](#warp-socks5-or-interface-split-tunneling-templates-and-chatgpt-unlock-methods)
- [WARP+ License and ID Acquisition](#warp-license-and-id-acquisition)
- [How to Obtain WARP Teams and Use on Linux](#how-to-obtain-warp-teams-and-use-on-linux)
- [Principle of WARP](#principle-of-warp)
- [Acknowledgments to Contributors and Global Service Status List](#acknowledgments-to-contributors-and-global-service-status-list)

---

## Changelog

**2025.03.24** menu.sh v3.1.5

1. Fixed the client’s WARP mode (network interface) not working after reboot;
2. Corrected the regex for Team IPv6 detection.

**2024.12.24** menu.sh v3.1.4 / warp-go.sh v1.2.3

- Support for Docker to listen on 0.0.0.0/0 externally without requiring host network mode. Thanks to @Anthony\_Tel.

**2024.9.24** menu.sh v3.1.3

- Added MASQUE protocol option to the Linux Client, available in both Proxy mode (menu 5) and WarpProxy mode (menu 14).

* **2024.9.14** menu.sh v3.1.2 / warp-go.sh v1.2.2

  1. Removed license generation from the previous version because cloning Warp+ licenses is officially prohibited;
  2. Removed unnecessary Python3 dependency.

* **2024.7.25** menu.sh v3.1.1 / warp-go.sh v1.2.1

  1. Support for self-hosted WARP API at [https://warp.cloudflare.now.cc/?run=pluskey](https://warp.cloudflare.now.cc/?run=pluskey) to generate a 1920 PB WARP+ license for Plus upgrade;
  2. Client lacked sufficient WARP+ support (IPv4 only, no IPv6);
  3. Optimized installer to reduce script runtime.

* **2024.7.18** menu.sh v3.1.0 / warp-go.sh v1.2.0

  1. Use self-built WARP API to upgrade to Teams account without needing a pre-obtained token (enter organization, email, verification code during script run);
  2. Client upgrade to Teams account omitted due to potential VPS disconnection risks—users can configure manually.

* **2024.7.8** menu.sh v3.0.10 / warp-go.sh v1.1.9

  1. Launched WARP API for account registration, Zero Trust, account info, etc.;
  2. Scripts to update the WARP API.

* **2024.6.30** menu.sh v3.0.9

  1. Multithreading for MTU/endpoint optimization, downloading wireguard-go, and dependencies reduced runtime by over 50%;
  2. Reverse-proxied ip-api.com/json and hits.seeyoufarm.com via Cloudflare Worker for better dual-stack support and faster fetch;
  3. DNS priority: Cloudflare 1.1.1.1 > Google 8.8.8.8.

* **2024.6.28** menu.sh v3.0.8

  - Official WARP Linux Client supports ARM64 and offers both SOCKS5 proxy and Warp interface modes.

* **2024.6.2** menu.sh v3.0.7

  - Support for CentOS 9, Alma Linux 9, Rocky Linux 9.

* **2024.5.5** menu.sh v3.0.6 / warp-go.sh v1.1.8

  - Support for Alpine edge.

* **2024.5.1** menu.sh v3.0.5

  - Handled apt repository changes for Debian 10 WireGuard-tools.

* **2024.4.14** menu.sh v3.0.4

  1. Alpine: check and update wget version;
  2. Added feedback message when warp connection fails.

* **2024.3.21** menu.sh v3.0.3 / warp-go.sh v1.1.7

  1. Updated commands per warp-cli; 2. Removed GitHub CDN.

* **2024.2.7** menu.sh v3.0.2

  - Checks for loaded WireGuard kernel module; loads it if missing.

* **2023.12.19** menu.sh v3.0.1 / warp-go.sh v1.1.6

  - Added UDP connectivity check; aborts if all WARP endpoints are unreachable.

... (Earlier entries truncated for brevity)

## Script Features

- Supports WARP+ accounts, includes third-party WARP+ traffic top-up and BBR kernel upgrade scripts.
- User-friendly menu for novices; advanced users can quickly deploy via suffix options.
- Auto-detects VPS OS: Ubuntu 16.04/18.04/20.04; Debian 9/10/11; CentOS 7/8; Alpine; Arch Linux (LTS recommended).
- Auto-detects hardware architecture: AMD, ARM, s390x.
- Auto-selects best WireGuard setup based on Linux version and virtualization:
  - Performance: Kernel-integrated WireGuard > kernel module > BoringTun > wireguard-go.
- Auto-detects latest WGCF GitHub release.
- Auto-configures WGCF based on public/private IP analysis.
- Prompts whether to apply WARP IP and shows IP geolocation.

## Benefits of WARP

- Supports chatGPT, unlocks Netflix streaming.
- Avoids Google CAPTCHA or academic search verification.
- Provides IPv4 interface for projects like QingLong and V2P.
- Enables bidirectional traffic for VPS jump-host and probing, replacing HE tunnelbroker.
- Allows IPv6-only VPS to support Telegram nodes.
- IPv6 nodes usable on IPv4-only clients like PassWall, ShadowSocksR Plus+.

## warp Run Script

**First run:**

```bash
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh [option] [license/url/token]
```

**Subsequent runs:**

```bash
warp [option] [license]
```

| Option & Args    | Description                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------- |
| h                | Help                                                                                         |
| 4                | Switch to WARP IPv4                                                                          |
| 4 license name   | Add WARP+ license and device name, e.g. `bash menu.sh 4 N5670ljg-sS9jD334-6o6g4M9F Goodluck` |
| 6                | Switch to WARP IPv6                                                                          |
| d                | Switch to WARP dual-stack                                                                    |
| o                | Toggle WARP on/off                                                                           |
| u                | Uninstall WARP                                                                               |
| n                | Refresh WARP network when disconnected (bug workaround)                                      |
| b                | Upgrade kernel, enable BBR and DUAL CC                                                       |
| a                | Upgrade free WARP account to WARP+                                                           |
| a license        | Add WARP+ license                                                                            |
| p                | Top-up WARP+ traffic                                                                         |
| c                | Install WARP Linux Client in SOCKS5 proxy mode                                               |
| l                | Install WARP Linux Client in WARP interface mode                                             |
| c license        | Install Client and add WARP+ license                                                         |
| r                | Toggle WARP Linux Client on/off                                                              |
| v                | Sync script to latest version                                                                |
| i                | Change WARP IP                                                                               |
| e                | Install iptables + dnsmasq + ipset split-streaming for media                                 |
| w                | Install WireProxy solution                                                                   |
| y                | Toggle WireProxy on/off                                                                      |
| k                | Switch between wireguard kernel and wireguard-go-reserved                                    |
| g                | Switch global/non-global mode or initial non-global install                                  |
| s 4/6/d          | Switch WARP priority IPv4/IPv6/default                                                       |
| (others or none) | Show menu                                                                                    |

\*Example: To add WARP dual-stack to an Oracle IPv4 server:

```bash
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh d
```

Refresh Japan Netflix IP:

```bash
warp i jp
```

## warp-go Run Script

**First run:**

```bash
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/warp-go.sh && bash warp-go.sh [option] [license]
```

**Subsequent runs:**

```bash
warp-go [option] [license]
```

| Option & Args    | Description                        |
| ---------------- | ---------------------------------- |
| h                | Help                               |
| 4                | Switch to WARP IPv4                |
| 4 license name   | Add WARP+ license and device name  |
| 6                | Switch to WARP IPv6                |
| d                | Switch to WARP dual-stack          |
| o                | Toggle warp-go on/off              |
| u                | Uninstall warp-go                  |
| a                | Upgrade free WARP account to WARP+ |
| a license name   | Add WARP+ license and device name  |
| v                | Sync script to latest version      |
| (others or none) | Show menu                          |

## Cloudflare API

### CLI API Usage

Access via browser or `curl` to execute WARP API requests:

| Run Parameter | Description                    | Params                              | Example                                                                                      |
| ------------- | ------------------------------ | ----------------------------------- | -------------------------------------------------------------------------------------------- |
|               | Usage guide                    |                                     | `https://warp.cloudflare.now.cc/`                                                            |
| `register`    | Register new device            | `team_token` (optional), `format`   | `https://warp.cloudflare.now.cc/?run=register&team_token=<Your-Team-Token>&format=json`      |
| `device`      | Retrieve device details        | `device_id`, `token`                | `https://warp.cloudflare.now.cc/?run=device&device_id=<ID>&token=<Token>`                    |
| `app`         | Get client configuration       | `token`                             | `https://warp.cloudflare.now.cc/?run=app&token=<Token>`                                      |
| `bind`        | Bind device to account         | `device_id`, `token`                | `https://warp.cloudflare.now.cc/?run=bind&device_id=<ID>&token=<Token>`                      |
| `name`        | Set device name                | `device_id`, `token`, `device_name` | `https://warp.cloudflare.now.cc/?run=name&device_id=<ID>&token=<Token>&device_name=<Name>`   |
| `license`     | Set device license             | `device_id`, `token`, `license`     | `https://warp.cloudflare.now.cc/?run=license&device_id=<ID>&token=<Token>&license=<License>` |
| `unbind`      | Unbind device                  | `device_id`, `token`                | `https://warp.cloudflare.now.cc/?run=unbind&device_id=<ID>&token=<Token>`                    |
| `cancel`      | Cancel device registration     | `device_id`, `token`                | `https://warp.cloudflare.now.cc/?run=cancel&device_id=<ID>&token=<Token>`                    |
| `id`          | Convert Client ID and Reserved | `convert`                           | `https://warp.cloudflare.now.cc/?run=id&convert=<4-char-string>`                             |
| `token`       | Obtain Zero Trust token        | `organization`, `email`, `code`     | Step1: request with org & email; Step2: include code values                                  |
| `key`         | Generate WireGuard key pair    | `format`                            | `https://warp.cloudflare.now.cc/?run=key&format=json`                                        |

### Shell API Usage

```bash
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/api.sh && bash api.sh [option]
```

| Option        | Description                                               |
| ------------- | --------------------------------------------------------- |
| -h/--help     | Help                                                      |
| -f/--file     | Save registration info to file (supports various clients) |
| -r/--register | Register account                                          |
| -t/--token    | Register with team token                                  |
| -d/--device   | Get registration info including Plus traffic              |
| -a/--app      | Get app info                                              |
| -b/--bind     | Get bound device info                                     |
| -n/--name     | Modify device name                                        |
| -l/--license  | Modify license                                            |
| -u/--unbind   | Unbind device                                             |
| -c/--cancel   | Cancel account                                            |
| -i/--id       | Show Client ID and Reserved conversion                    |

## Method to Unlock Netflix with WARP IP

- Use a dedicated one-click script: [unlock\_warp](https://github.com/fscarmen/unlock_warp)
- Example: unlock Hong Kong (`hk`):
  ```bash
  warp i hk
  ```
  Run under `screen` or `nohup` for background operation.
- If unable to get an unlock IP after a long time, check Cloudflare status: [https://www.cloudflarestatus.com/](https://www.cloudflarestatus.com/)

## WARP SOCKS5 or Interface Split Tunneling Templates and chatGPT Unlock Methods

```json
{
  "outbounds":[
    { "protocol":"freedom" },
    { "tag":"warp","protocol":"socks","settings":{"servers":[{"address":"127.0.0.1","port":40000}]} },
    { "tag":"WARP-socks5-v4","protocol":"freedom","settings":{"domainStrategy":"UseIPv4"},"proxySettings":{"tag":"warp"} },
    { "tag":"WARP-socks5-v6","protocol":"freedom","settings":{"domainStrategy":"UseIPv6"},"proxySettings":{"tag":"warp"} }
  ],
  "routing":{"rules":[
    { "type":"field","domain":["geosite:openai","ip.gs"],"outboundTag":"WARP-socks5-v4" },
    { "type":"field","domain":["geosite:google","geosite:netflix","p3terx.com"],"outboundTag":"WARP-socks5-v6" }
  ]}
}
```

```json
{
  "outbounds":[
    { "protocol":"freedom" },
    { "tag":"WARP-interface-v4","protocol":"freedom","settings":{"domainStrategy":"UseIPv4"},
      "streamSettings":{"sockopt":{"interface":"CloudflareWARP","tcpFastOpen":true}} },
    { "tag":"WARP-interface-v6","protocol":"freedom","settings":{"domainStrategy":"UseIPv6"},
      "streamSettings":{"sockopt":{"interface":"CloudflareWARP","tcpFastOpen":true}} }
  ],
  "routing":{"domainStrategy":"AsIs","rules":[
    { "type":"field","domain":["geosite:google","geosite:openai","ip.gs"],"outboundTag":"WARP-interface-v4" },
    { "type":"field","domain":["geosite:netflix","p3terx.com"],"outboundTag":"WARP-interface-v6" }
  ]}
}
```

```json
{
  "outbounds":[
    { "protocol":"freedom","tag":"direct" },
    { "protocol":"wireguard","settings":{ /* paste your private_key, address, peers, reserved, mtu */ },"tag":"wireguard" },
    { "protocol":"freedom","settings":{"domainStrategy":"UseIPv4"},"proxySettings":{"tag":"wireguard"},"tag":"warp-IPv4" },
    { "protocol":"freedom","settings":{"domainStrategy":"UseIPv6"},"proxySettings":{"tag":"wireguard"},"tag":"warp-IPv6" }
  ],
  "routing":{"domainStrategy":"AsIs","rules":[
    { "type":"field","domain":["geosite:openai","ip.gs"],"outboundTag":"warp-IPv4" },
    { "type":"field","domain":["geosite:netflix","p3terx.com"],"outboundTag":"warp-IPv6" }
  ]}
}
```

## WARP+ License and ID Acquisition

See Argo 2.0 official intro: [Argo 2.0: Smart Routing Learns New Tricks](https://blog.cloudflare.com/argo-v2/)

> In practice, WARP+ doesn’t differ in speed from free version on non-CF sites; it only routes Cloudflare-hosted sites via Argo-like optimizations, while free routes directly to origin. — Luminous

## How to Obtain WARP Teams and Use on Linux

- [https://warp-token.cloudflare.now.cc/](https://warp-token.cloudflare.now.cc/) (Fscarmen’s)
- [https://web--public--warp-team-api--coia-mfs4.code.run/](https://web--public--warp-team-api--coia-mfs4.code.run/) (Coia’s)

## Principle of WARP

WARP is a Cloudflare service based on WireGuard for secure and accelerated traffic via CF edge nodes, providing NAT-based IPv4/IPv6 addresses. It enables:

- IPv4-only servers to access IPv6 networks by routing IPv6 through WARP interface.
- IPv6-only servers to access IPv4 networks by routing IPv4 through WARP interface.
- Dual-stack servers to selectively route traffic for privacy or to bypass restrictions.

Performance ranking: Kernel-integrated > kernel module > wireguard-go.

## Acknowledgments to Contributors and Global Service Status List

**Projects & Articles (in no particular order):**

- P3terx: Cloudflare WARP for VPS extra IPv4/IPv6 support
- Oreomeow, Luminous, Hiram, Cloudflare, WireGuard authors, Parker C. Stephens, Anemone, wangying202, LUBAN, valetzx, badafans, chika0801, xXcmd1152Xx, ylx2016, ALIILAPRO, mixool, luoxue-bot, lmc999, WireProxy author, etc.

**Service Providers & APIs:**

- fscarmen’s Warp API & Zero Trust Token API
- Cloudflare Warp(+): [https://1.1.1.1/](https://1.1.1.1/)
- WGCF original: [https://github.com/ViRb3/wgcf/](https://github.com/ViRb3/wgcf/)
- warp-go: [https://gitlab.com/ProjectWARP/warp-go](https://gitlab.com/ProjectWARP/warp-go)
- WireGuard-GO: [https://git.zx2c4.com/wireguard-go/](https://git.zx2c4.com/wireguard-go/)
- IP lookup: ifconfig.co, ip.gs, ip.sb, ip-api.com
- Traffic stats: [https://hits.seeyoufarm.com/](https://hits.seeyoufarm.com/)

**Global WARP Service Status:**

- Operational = Normal; Re-routed = Maintenance: [https://www.cloudflarestatus.com/](https://www.cloudflarestatus.com/)


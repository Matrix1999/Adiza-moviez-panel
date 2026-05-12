<div align="center">

    <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=200&section=header&text=Adiza%20Moviez%20Panel&fontSize=48&fontColor=fff&animation=twinkling&fontAlignY=40&desc=License%20%7C%20Admin%20%7C%20Auto-Update&descAlignY=60&descSize=18" width="100%"/>

    <br/>

    ![Version](https://img.shields.io/badge/Worker-v4.1-red?style=for-the-badge&logo=server&logoColor=white)
    ![Platform](https://img.shields.io/badge/Platform-Secure%20Cloud%20API-orange?style=for-the-badge&logo=server)
    ![App](https://img.shields.io/badge/App-Android%20Flutter-blue?style=for-the-badge&logo=flutter)
    ![Security](https://img.shields.io/badge/Security-HMAC--SHA256-green?style=for-the-badge&logo=shield)
    ![Access](https://img.shields.io/badge/Access-Private%20VIP-red?style=for-the-badge)

    <br/>

    > **The official admin panel, license backend, and auto-update system for Adiza Moviez Box.**
    > Manages device activations, Telegram bot commands, group membership enforcement, and signed app updates.

    <br/>

    [![Telegram Group](https://img.shields.io/badge/Join%20Community-Telegram-blue?style=for-the-badge&logo=telegram)](https://t.me/+rvI8wgFwCiA3NWJk)
    [![Channel](https://img.shields.io/badge/Channel-reversemoda-purple?style=for-the-badge&logo=telegram)](https://t.me/reversemoda)
    [![Chatroom](https://img.shields.io/badge/Chatroom-Reversal_X_Mods-blueviolet?style=for-the-badge&logo=telegram)](https://t.me/reversalxmods1)

  </div>

  ---

  ## ✨ Features

  | Feature | Description |
  |---------|-------------|
  | 🔐 **Device Licensing** | Activate / revoke / expire Android devices per app |
  | 🤖 **Telegram Bot** | Users activate with `/adiza-moviez-box <code>` in the group |
  | 👥 **Group Enforcement** | Auto-checks group membership on every license ping |
  | ⏱ **Expiry System** | Per-device expiry — `30d` `2mo` `lifetime` |
  | 📲 **Auto-Update** | App polls `adiza.moviz.box.json` here and self-updates |
  | 🛡 **Request Signing** | Every request signed — unsigned calls are rejected |
  | ⚡ **Anti-Replay** | 5-minute nonce + timestamp window on every check |
  | 🔒 **Signed Cache** | Local cache is cryptographically signed — tampering is detected |
  | 🕵 **Hook Detection** | Injection tools are detected and silently block access |
  | ⏰ **Clock Guard** | Rolling back the device clock is detected and blocked |
  | 📊 **Web Panel** | Full device management dashboard (admin only) |
  | 📢 **Announcements** | Rotating banner system sent to the Telegram group |
  | 🌐 **Multi-App** | One backend, multiple apps via `app_id` |

  ---

  ## 🤖 Telegram Bot Commands

  ### User Flow (inside the group)

  ```
  1. User opens Adiza Moviez Box → copies their Device Code
  2. User sends:  /adiza-moviez-box <device_code>
  3. Bot confirms → app unlocks automatically within 3 seconds
  ```

  ### Admin Commands

  | Command | Example | Description |
  |---------|---------|-------------|
  | `/activate <app> <device> <user> [expiry]` | `/activate adiza-moviez-box abc123 john 1mo` | Force-activate |
  | `/revoke <app> <device>` | `/revoke adiza-moviez-box abc123` | Disable device |
  | `/restore <app> <device>` | `/restore adiza-moviez-box abc123` | Re-enable device |
  | `/delete <app> <device>` | `/delete adiza-moviez-box abc123` | Remove permanently |
  | `/renewall <app> [expiry]` | `/renewall adiza-moviez-box 2mo` | Bulk-renew all users |
  | `/check <device_or_@user>` | `/check abc123` | Look up device status |
  | `/list [app]` | `/list adiza-moviez-box` | Paginated device list |
  | `/stats` | — | System totals |
  | `/announce <app>` | `/announce adiza-moviez-box` | Send banner to group |

  ### Expiry Formats

  ```
  30m · 2h · 7d · 2w · 1mo · 3mo · YYYY-MM-DD · 2080 (lifetime)
  ```

  ---

  ## 🔄 Auto-Update System

  The app checks **`adiza.moviz.box.json`** (this repo) on every launch and every 30 min in the background.

  ### Manifest Fields

  ```json
  {
    "version_code": 6,
    "version_name": "4.2.0",
    "changelog": "• What changed\n• Another improvement",
    "apk_url": "https://github.com/Matrix1999/adiza-moviez-box-flutter/releases/download/v4.2.0/AdizaMoviezBox-arm64-v8a-release.apk",
    "update_size": "48.5 MB",
    "force_update": false
  }
  ```

  ### Release Checklist

  - [ ] Build APK via GitHub Actions in `adiza-moviez-box-flutter`
  - [ ] Create a GitHub Release and attach the APK
  - [ ] Edit `adiza.moviz.box.json` in this repo: bump `version_code`, update `apk_url`
  - [ ] Commit — app picks it up within 30 min automatically
  - [ ] Set `"force_update": true` if this release is mandatory

  ---

  ## 🛡 Security Hardening

  ### Protection Layers

  | Attack | Defense |
  |--------|---------|
  | Request forgery | Cryptographic signature on every call |
  | Replay attack | Server rejects requests older than 5 minutes |
  | Response injection | Server-signed response — mismatch blocks access |
  | SharedPrefs tampering | Daily-expiring cryptographic signature on cached result |
  | Injection / hook tools | Process map scan + port probe → silent block |
  | Clock rollback | Cached server timestamp — rollback triggers block |
  | Admin API abuse | Constant-time secret check — fail-closed if unset |
  | Package spoofing | Backend rejects wrong package names |

  ---

  <div align="center">

  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=120&section=footer&animation=twinkling" width="100%"/>

  **Built with ❤️ by Reversal X Mods**

  [![Support](https://img.shields.io/badge/Support-@reversalxmods1-blue?style=flat-square&logo=telegram)](https://t.me/reversalxmods1)

  *逆转 X 模组*

  </div>
  
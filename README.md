<div align="center">

  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=200&section=header&text=Adiza%20Moviez%20Panel&fontSize=48&fontColor=fff&animation=twinkling&fontAlignY=40&desc=License%20%7C%20Admin%20Worker%20%7C%20Auto-Update&descAlignY=60&descSize=18" width="100%"/>

  <br/>

  ![Version](https://img.shields.io/badge/Worker-v4.1-red?style=for-the-badge&logo=cloudflare&logoColor=white)
  ![Platform](https://img.shields.io/badge/Platform-Cloudflare%20Workers-orange?style=for-the-badge&logo=cloudflare)
  ![DB](https://img.shields.io/badge/Database-Cloudflare%20D1-yellow?style=for-the-badge&logo=cloudflare)
  ![App](https://img.shields.io/badge/App-Android%20Flutter-blue?style=for-the-badge&logo=flutter)
  ![Security](https://img.shields.io/badge/Security-HMAC--SHA256-green?style=for-the-badge&logo=shield)
  ![License](https://img.shields.io/badge/Access-Private%20VIP-red?style=for-the-badge)

  <br/>

  > **The official admin panel, license backend, and auto-update system for Adiza Moviez Box.**
  > Built on Cloudflare Workers + D1. Manages device activations, Telegram bot commands, group membership enforcement, and signed app updates.

  <br/>

  [![Telegram Group](https://img.shields.io/badge/Join%20Community-Telegram-blue?style=for-the-badge&logo=telegram)](https://t.me/+rvI8wgFwCiA3NWJk)
  [![Channel](https://img.shields.io/badge/Channel-reversemoda-purple?style=for-the-badge&logo=telegram)](https://t.me/reversemoda)
  [![Chatroom](https://img.shields.io/badge/Chatroom-Reversal_X_Mods-blueviolet?style=for-the-badge&logo=telegram)](https://t.me/reversalxmods1)

  </div>

  ---

  ## 📑 Table of Contents

  - [Architecture](#-architecture)
  - [Features](#-features)
  - [Worker Setup](#-worker-setup)
  - [Environment Variables](#-environment-variables)
  - [Telegram Bot Commands](#-telegram-bot-commands)
  - [Auto-Update System](#-auto-update-system)
  - [Security Hardening](#-security-hardening)
  - [API Reference](#-api-reference)

  ---

  ## 🏗 Architecture

  ```
  ┌─────────────────────────────────────────────────────────┐
  │                 Adiza Moviez Box App                    │
  │            Flutter · Android · ARM64                    │
  └────────────────────┬────────────────────────────────────┘
                       │  HMAC-SHA256 signed requests
                       ▼
  ┌─────────────────────────────────────────────────────────┐
  │         Cloudflare Worker  (uganda-munowatch)           │
  │    https://uganda-munowatch.matrixzat99.workers.dev     │
  ├─────────────────────────────────────────────────────────┤
  │  /api/vip/check-direct  → license verification          │
  │  /api/vip/token         → activation token issue        │
  │  /api/time              → clock tamper detection        │
  │  /api/admin/*           → admin panel (key-protected)   │
  │  /webhook               → Telegram bot commands         │
  │  /panel                 → web admin dashboard           │
  └──────────┬──────────────────────────┬───────────────────┘
             │                          │
             ▼                          ▼
  ┌──────────────────┐      ┌───────────────────────┐
  │  Cloudflare D1   │      │   Telegram Bot API    │
  │  activations     │      │   Group membership +  │
  │  app_activations │      │   user notifications  │
  │  vip_tokens      │      └───────────────────────┘
  │  apps · settings │
  └──────────────────┘

  Adiza-moviez-panel (this repo)
    ├── adiza.moviz.box.json   ← auto-update manifest (app polls this)
    └── cloudflare/worker.js  ← full worker source
  ```

  ---

  ## ✨ Features

  | Feature | Description |
  |---------|-------------|
  | 🔐 **Device Licensing** | Activate / revoke / expire Android devices per app |
  | 🤖 **Telegram Bot** | Users activate with `/adiza-moviez-box <code>` in the group |
  | 👥 **Group Enforcement** | Auto-checks group membership on every license ping |
  | ⏱ **Expiry System** | Per-device expiry — `30d` `2mo` `lifetime` |
  | 📲 **Auto-Update** | App polls `adiza.moviz.box.json` here and self-updates |
  | 🛡 **HMAC Signing** | Every request signed — unsigned calls rejected with 403 |
  | ⚡ **Anti-Replay** | 5-minute nonce + timestamp window on every check |
  | 🔒 **Signed Cache** | Local cache is HMAC-signed daily — writing `true` won't work |
  | 🕵 **Hook Detection** | Frida / Xposed kills the app silently before any check runs |
  | ⏰ **Clock Guard** | Cached server time — rolling back the device clock is detected |
  | 📊 **Web Panel** | `/panel?secret=XXX` — full device management dashboard |
  | 📢 **Announcements** | 6-banner rotating system sent to the Telegram group |
  | 🌐 **Multi-App** | One worker, multiple apps via `app_id` |

  ---

  ## ⚙ Worker Setup

  ### 1. Prerequisites

  - Cloudflare account (Workers + D1 plan)
  - A Telegram Bot from [@BotFather](https://t.me/BotFather)
  - Wrangler CLI installed: `npm install -g wrangler`

  ### 2. Create D1 Database

  ```bash
  wrangler d1 create adiza-munowatch
  # Copy the database_id from the output
  ```

  ### 3. wrangler.toml

  ```toml
  name = "uganda-munowatch"
  main = "cloudflare/worker.js"
  compatibility_date = "2024-01-01"

  [[d1_databases]]
  binding      = "DB"
  database_name = "adiza-munowatch"
  database_id  = "YOUR_D1_DATABASE_ID"
  ```

  ### 4. Deploy

  ```bash
  wrangler deploy
  ```

  ### 5. Register Webhook

  ```
  https://api.telegram.org/bot<BOT_TOKEN>/setWebhook?url=https://uganda-munowatch.matrixzat99.workers.dev/webhook
  ```

  ---

  ## 🔑 Environment Variables

  Set in **Cloudflare Dashboard → Workers → uganda-munowatch → Settings → Variables → Encrypt**:

  | Variable | Required | Description |
  |----------|:--------:|-------------|
  | `BOT_TOKEN` | ✅ | Telegram Bot token from @BotFather |
  | `ADMIN_SECRET` | ✅ | Strong password for admin API & web panel |
  | `APP_HMAC_SECRET` | ✅ | `RXMads93Kz7wPqLmVb2eN5fYcA1jTu8h` — must match app build |
  | `RESP_SIGN_SECRET` | ✅ | `pG6vHsE4nWxK9mBdJ3rQyUoZ2cIlFaT7` — signs responses |
  | `GROUP_ID` | ✅ | Telegram group chat ID (negative number, e.g. `-1001234567890`) |
  | `CHANNEL_ID` | ☑️ | Telegram channel ID for announcements |
  | `MUNO_EMAIL` | ☑️ | Munowatch account email |
  | `MUNO_PASS` | ☑️ | Munowatch account password |

  > ⚠️ **Never commit secrets to this repo.** Set them only via the Cloudflare dashboard.

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
  - [ ] Create a GitHub Release there and attach the APK
  - [ ] Edit `adiza.moviz.box.json` **in this repo**: bump `version_code`, update `apk_url`
  - [ ] Commit — app picks it up within 30 min automatically
  - [ ] Set `"force_update": true` if this release is mandatory

  ---

  ## 🛡 Security Hardening

  ### Protection Layers

  | Attack | Defense |
  |--------|---------|
  | Request forgery | HMAC-SHA256 of `nonce|ts|device_id|pkg` on every call |
  | Replay attack | Server rejects requests older than 5 minutes |
  | Response injection (Frida) | Server-signed response HMAC — mismatch = deny |
  | SharedPrefs tampering | Daily-expiring HMAC on cached result |
  | Frida hook | `/proc/self/maps` scan + port 27042 probe → `exit(0)` |
  | Xposed / LSPosed | Same `/proc/self/maps` scan → `exit(0)` |
  | Clock rollback | Cached server timestamp — rollback triggers block |
  | Admin API abuse | Constant-time ADMIN_SECRET check — fail-closed if unset |
  | Package spoofing | Worker rejects `pkg ≠ com.adiza.moviezbox` |

  ### HMAC Request / Response Flow

  ```
  App                                      Worker
   │                                         │
   ├─ nonce  = crypto.randomBytes(16)        │
   ├─ ts     = unix seconds now             │
   ├─ sig    = HMAC-SHA256(                 │
   │               nonce|ts|device|pkg,      │
   │               APP_HMAC_SECRET)          │
   │                                         │
   │──── POST /api/vip/check-direct ────────▶│
   │     {device_id, pkg, nonce, ts, sig}    │── reject if sig invalid / ts stale
   │                                         │── lookup DB
   │                                         │── check group membership
   │                                         │── rsig = HMAC-SHA256(
   │                                         │       active|ts|nonce,
   │                                         │       RESP_SIGN_SECRET)
   │◀─── {active, ts, nonce, sig: rsig} ────│
   │                                         │
   ├─ verify rsig                            │
   └─ hook injection = mismatch = deny       │
  ```

  ---

  ## 📡 API Reference

  | Method | Path | Auth | Description |
  |--------|------|------|-------------|
  | `GET` | `/api/time` | None | Server timestamp |
  | `GET` | `/api/vip/token` | App secret | Issue activation token |
  | `POST` | `/api/vip/check-direct` | HMAC | Check if device is active |
  | `GET` | `/api/status` | None | Legacy polling status |
  | `GET|POST` | `/api/admin/*` | ADMIN_SECRET | Admin CRUD operations |
  | `POST` | `/webhook` | Telegram | Bot event receiver |
  | `GET` | `/panel` | ADMIN_SECRET | Web admin dashboard |

  ---

  <div align="center">

  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=120&section=footer&animation=twinkling" width="100%"/>

  **Built with ❤️ by Reversal X Mods**

  [![Support](https://img.shields.io/badge/Support-@reversalxmods1-blue?style=flat-square&logo=telegram)](https://t.me/reversalxmods1)

  *逆转 X 模组*

  </div>
  
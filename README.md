# Wireless Arcade — ESP32 Local Wi-Fi Arcade, 8 Games, No Internet (Browser Web Flasher)

**AutoMate Vol.1 · LPA-03 · Age 8+**

Turn an ESP32-S3 into a self-contained retro arcade. It broadcasts its own
Wi-Fi network; anyone nearby joins with a phone and a menu of **eight 8-bit
games** opens automatically — Serpent, Paddle Duel, Brick Smash, Block Drop,
Muncher, Hopper, Star Raider, Tower. Scores are kept on the device in a
permanent **Hall of Fame**, and beating one unlocks gold/rainbow skins across
every game on your next visit. No app, no accounts, no internet — it works
the same in a dead-signal basement as it does anywhere else.

This repository hosts the **browser-based installer** only. Buy the product on
Gumroad, then flash your ESP32-S3 straight from Chrome or Edge — open
`install.html`, enter your license key, click, done. No build tools, no
`idf.py`, no drivers.

It exists solely to host the files a browser needs to flash a device over USB
(ESP Web Tools requires them to be fetchable over HTTP/HTTPS). It is **not** an
invitation to use the firmware without a purchase — see `LICENSE.txt`.

- **Buy:** _Gumroad listing link — coming soon_
- **Flash (after purchase):** [whoishafiz.github.io/Wireless_Arcade/install.html](https://whoishafiz.github.io/Wireless_Arcade/install.html)

> **Status:** the license-verification relay is not wired up yet, so the
> flasher page renders but the gate will not unlock until the Gumroad listing
> and Cloudflare Worker are live. `install.html` and `cloudflare-worker.js`
> ship with clearly-marked placeholders (`WORKER_URL`, `GUMROAD_PRODUCT_ID`,
> the buy link) until then.

## Getting started

1. Purchase on Gumroad → instantly receive a license key + the full ESP-IDF
   source as a zip.
2. Flash via browser: open `install.html` (Chrome or Edge on desktop), enter
   your license key, plug in your ESP32-S3, click **Connect & Install**, and
   hold the **BOOT** button until writing starts.
3. Power on → on a phone, join the Wi-Fi network **`ARCADE 01`** → the game
   menu opens automatically. If it doesn't within a few seconds: on iOS, check
   Settings → Wi-Fi → ⓘ next to the network → make sure **Auto-Login** is on
   (it can switch off if the phone has joined this SSID before); otherwise
   open a browser and go to `http://192.168.4.1`.

## What's on the device

- **8 games**, all served from the chip, playable on any phone browser:
  Serpent, Paddle Duel (head-to-head), Brick Smash, Block Drop, Muncher,
  Hopper, Star Raider, Tower.
- **Hall of Fame** — a top-10 leaderboard per game stored in the ESP32's
  flash. It survives reboots and power loss. Reconnect any time to see it.
- **Player perks** — once your name is on a board, gold/rainbow skins unlock
  across all 8 games on your next connection.
- Retro green-CRT UI, on-screen name entry, GAMES / SCORES / CREDITS tabs.

## Hardware required

- **An ESP32-S3 board (4 MB flash minimum).** Hardware-verified on the
  ESP32-S3 Super Mini.
- USB cable + a power source — a power bank is fine once it's placed.
- Chrome or Edge on desktop for the one-time flash. Players only need a phone
  with Wi-Fi and a browser.

## Building it yourself

Prefer to build from source or customize the games / branding first? Your
Gumroad download is a complete ESP-IDF (C) project — `main/`, `CMakeLists.txt`,
`sdkconfig.defaults`. Built and verified with ESP-IDF v5.5.2. See `LICENSE.txt`
for what you're allowed to do with the source.

## Files in this repository

| File | Purpose |
|---|---|
| `install.html` | Browser flasher page + license-key gate UI |
| `manifest.json` | ESP Web Tools manifest — chip family + flash offsets |
| `bootloader.bin` / `partition-table.bin` / `firmware.bin` | Build output, offsets `0x0` / `0x8000` / `0x10000` (ESP32-S3) |
| `cloudflare-worker.js` | CORS relay to Gumroad's license-verify endpoint |
| `LICENSE.txt` | Terms — flasher page reusable, `.bin` files are not |
| `robots.txt` | Blocks search-engine indexing of the install page |

## Deploying the flasher (maintainer notes)

1. Enable GitHub Pages on this repo (Settings → Pages → deploy from `main`).
2. Create the Gumroad listing, enable per-sale license keys, attach the
   source zip, and read the real `product_id` from Gumroad's verify-endpoint
   error message (see the header comment in `cloudflare-worker.js`).
3. Paste `product_id` into `cloudflare-worker.js`, deploy the Worker, then
   paste its URL into `WORKER_URL` and the real buy link into `#buy-link` in
   `install.html`.
4. Commit, push, and test end-to-end with a real license key.

Until the placeholders are real, `install.html` renders fine but the gate
never unlocks — the safe failure mode.

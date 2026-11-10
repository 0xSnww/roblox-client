<img src="assets/icon.png" width="64" height="64" alt="Orbit">

# Orbit

A fast, native Roblox launcher for Windows, written in Rust. No webview, no Electron.

## Download

Get **orbit.exe** from the [latest release](https://github.com/0xSnww/roblox-client/releases/latest) and run it. There's no installer; keep it wherever you like.

Orbit updates itself: when a new version is out it offers it in the app (Settings → About, or the pill in the title bar). Updates are signed, and Orbit refuses any that aren't. Settings → About can also put the previous version back.

Orbit is closed source. This repository only hosts its releases; the "Source code" archives GitHub attaches to each release contain just this README.

## Features

**Server regions & smart join**
- Server browser for any experience: datacenter city, region badge and estimated ping for every server, filter by region, sort by ping / players / FPS. Friends' avatars show on the servers they're in.
- Server locations come from a **bundled Roblox datacenter map** (offline, no third party sees your IP or the servers you look at); unknown address blocks fall back to IP geolocation, snapped to the nearest known datacenter.
- **Smart join**: scans servers and picks the best one for your rules: best ping, preferred **datacenters** and regions (ordered), strict mode, blocked datacenters/regions, max ping, party size, busy vs quiet. It stops as soon as it finds a server about as close as the map says is possible.
- **Smart join for roblox.com**: with Orbit as the link handler, the website's Play button lands you on the best server for your rules, too.
- **Wait in line**: joining a full server makes Orbit keep checking and launch you the moment a slot opens.
- Pin servers, copy invite links, paste any server id / share link / private-server link / game link to join.

**Bloxstrap / Fishstrap-style launcher (Windows)**
- Installs and updates Roblox itself from Roblox's CDN (mirror fallback, md5-verified while streaming, cached by hash; old versions are only pruned while Roblox is closed, and the previous one is kept).
- Registers as the `roblox-player:` handler so Play on roblox.com goes through Orbit (reversible, restores the original handler).
- Deployment channels, multi-instance (several Roblox windows, each tracked separately), repair install.
- **Mods** overlay folder, applied reversibly: removing a mod restores Roblox's original file.
- **Custom font**: pick a `.ttf` / `.otf` and every built-in Roblox font family uses it (removing it restores Roblox's own).
- FastFlags: curated presets clearly tagged when Roblox's allowlist may ignore them, plus a raw editor with type validation and JSON import/export.
- **Per-game settings**: for any experience, pick the account it launches as, its own server rules, FastFlags layered over your global ones, its own mods folder, and a saved **outfit** to wear while you play (your previous avatar is put back when you leave).
- Tames the Roblox app (opt-in): stop it adding itself to Windows startup, stop it staying in the tray. Both are undone when switched off.

**Everything else**
- Home with a featured carousel that runs under the title bar, live trending rows, the games you last played, favourites (star any game), and a card for every running client.
- **Your choice of navigation**: a slim icon rail, a sidebar that also shows online friends and pinned servers, or tabs in the title bar. Subtle motion throughout, with a switch to turn it off.
- Multiple accounts, three ways to sign in: **Quick Login** (scan a QR with your phone or enter a code on roblox.com), **import from the Roblox app** (Windows), or paste a `.ROBLOSECURITY` cookie. Sessions live in Windows Credential Manager / macOS Keychain and are checked in the background ("session expired" shows on the Accounts page). **Play as…** launches any game as any saved account.
- Friends with live presence, profiles, one-click join, and a notification when a friend starts playing. Status arrives over Roblox's **realtime connection** (the one roblox.com uses), so it is instant; polling is only the fallback.
- Play history with server location, account, crashes and per-region time stats.
- **In-game overlay** (Windows, opt-in): a HUD over the game made of widgets you choose and drag into place: a stats pill or list (ping, jitter, packet loss, server city, players, server FPS, Roblox's CPU and memory, session time, clock), a two-minute ping graph, and friend alerts and session events shown in the game. Size, background and outline are adjustable. A hotkey (Ctrl+Shift+F9 by default) opens an in-game menu to rejoin, hop servers, copy the join link, join a friend, edit the HUD or close Roblox; another (Ctrl+Shift+F10) hides the HUD. Both can be changed and only exist while Roblox is in front. It's a window of Orbit's own, click-through except while its menu is open.
- Discord Rich Presence with game, server city and a Join button; games that speak **BloxstrapRPC** can set the text and images.
- Runs **once per user** and can live in the **tray**: website Play clicks are handed to the running Orbit, so sessions, Discord and alerts keep working with the window closed. Optionally starts with Windows (hidden), hides to the tray when a game starts, and asks before quitting mid-game.
- **Command palette** (Ctrl+K): pages, recent and favourite games, friends, accounts, paste-a-link-to-join, search. Alt+←/→ and mouse back/forward navigate; Ctrl+F searches; Ctrl+, opens Settings.
- **Signed self-updates** from GitHub Releases, with one-click rollback to the previous version.
- **Save diagnostics**: a zip of the report, settings and logs with accounts, cookies and tickets removed.

## Notes & limits

- Server locations need a signed-in account: Roblox only returns a server's address to a logged-in player (via the join API). Ping in server lists is an estimate from distance to the datacenter; game servers don't answer ping.
- The overlay's ping is measured: the time of a TCP handshake to an HTTPS front end in the server's datacenter, every 2 seconds. It's the network round trip to that datacenter, not the in-game ping Roblox shows (which adds server frame time). Datacenters with no reachable front end show the estimate with a `~`.
- The overlay follows the most recently started client; with several clients open it doesn't yet know which window is which. Players and server FPS come from the public server list, so they're missing on private and reserved servers and in games with more than a few hundred servers. There's no client FPS: reading it needs either code inside Roblox or elevated tracing.
- Managed installs, protocol registration, multi-instance, the tray and the overlay are Windows-only. On macOS Orbit launches the installed Roblox.app; on Linux it hands launches to [Sober](https://sober.vinegarhq.org) when installed (untested).
- Orbit only uses supported surfaces (web APIs, `ClientAppSettings.json`, launch URIs, the client's own log). It never injects into or modifies the Roblox client; the overlay is a separate window drawn on top.


Logs: `%LOCALAPPDATA%\Orbit\logs` (seven days are kept; cookies, launch tickets and tokens are redacted before anything is written). **Settings → About → Save diagnostics** makes a zip to attach to a bug report, with accounts, cookies and tickets removed.

Not affiliated with Roblox Corporation.

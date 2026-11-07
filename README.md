# Wheelcast 🐹📺

**Stream overlays powered by Squeektoshi's live wheel.** Free, no login, no keys, no server.

Wheelcast turns [Squeekchain](https://squeekchain.com)'s live API into OBS browser sources. When Squeektoshi hops on his wheel, your stream knows. When the chain mines blocks, your chat celebrates. Viewers predict what he does next, and the real round on Sepolia decides who was right.

## Overlays

| Overlay | What it does | OBS size |
|---|---|---|
| 🔴 Run alert | Animated banner with Squeektoshi spinning his wheel the moment he starts running (`IDLE → SPINNING`). Optional nap alert. Synthesised squeak sound. | 800 × 220 |
| 🎛️ Live RPM gauge | Speedometer following his real wheel RPM, speed in m/s, distance run during your stream, current Squeek block. | 360 × 290 |
| ⛏️ Block celebration | Confetti and a mining banner whenever a settlement completes new Squeek blocks, with SQK minted and the round number. | 1920 × 1080 |
| 🗳️ Chat prediction | Viewers type `!run` or `!nap` in Twitch chat. When the next 10-minute round settles, the outcome is read from the chain (work added > 0 = he ran). Winners build streaks; top 3 streaks get a podium. | 380 × 330 |
| 📰 Stats ticker | Lower-third scrolling status, RPM, block progress, distance this stream, 24h work, blocks and SQK minted. | 1920 × 56 |

`overlay.html?w=all` shows every overlay laid out for a 1920 × 1080 scene.

## Use it

1. Open `index.html` (the settings page), enter your Twitch channel, pick a theme, size and sound.
2. Click **Copy OBS link** under an overlay.
3. OBS → Sources → **+** → **Browser**, paste the link, set the size from the card. Tick *Control audio via OBS* for the sounds.

The settings page shows live previews with demo data and has test buttons for every event.

### URL parameters (`overlay.html`)

| Param | Values | Default |
|---|---|---|
| `w` | `alert`, `gauge`, `block`, `poll`, `ticker`, `all` | `all` |
| `channel` | Twitch channel name (enables chat votes) | none |
| `theme` | `night`, `light`, `mint` | `night` |
| `scale` | 0.3 – 3 | 1 |
| `sound` | `0` to mute | on |
| `nap` | `1` to also alert when he stops | off |
| `secs` | alert duration, 4 – 30 | 9 |
| `demo` | `1` simulates a running hamster, chat and blocks | off |

## Data used

All from `https://api.squeekchain.com/v1` (public, CORS-enabled), polled from the viewer's OBS:

- `/status` every 5 s: `measurement.coreStatus`, `rpmMilli`, `speedMmPerSecond`, `distanceMm`; `builder.projected.blockHeight`, `blockProgress`; `builder.anchor.settledThroughRound`.
- `/settlements?range=24h` every 30 s: `recentSettlements` (new rounds → block celebrations and poll results, using `workAdded`, `completedBlockCount`, `lastCompletedBlock`, `sqkMintedBaseUnits`) and `summary` / `buckets` for the ticker.

Twitch chat is read anonymously over Twitch's public IRC WebSocket (`justinfan` read-only login). Wheelcast never posts and never asks for a token. Streaks are stored in the OBS browser's localStorage per channel.

Squeektoshi's wheel time is never forced or encouraged; naps are part of the show.

## Art

The hamster and the wheel are original illustrations made with Google Gemini, cut out and optimised to WebP.

## Squeekchain Genesis verification

<!-- Paste the repository line Squeekchain gives you below this comment -->

## Author

Made by serkan ([@serkankels](https://x.com/serkankels) on X).

## License

MIT

# Wheelcast

**Broadcast graphics for a hamster.** Five free OBS overlays that react to Squeektoshi's wheel and the Squeekchain rounds in real time. No login, no keys, no server.

Open the site, set your Twitch channel and look, copy a link, add it to OBS as a Browser source. The settings page has a live program monitor running on demo data, with buttons to take each overlay on or off air and to fire every event.

## Overlays

| Overlay | What it does | OBS size |
|---|---|---|
| Run alert | A broadcast wedge wipes in and Squeektoshi leaps out of it the moment the wheel sensor reports he is running (`coreStatus` IDLE → SPINNING), with his live rpm and the next block. Optional nap alert. | 800 × 220 |
| Wheel telemetry | F1-style rev lights that climb from green to red with his real rpm, plus speed in m/s, distance run during your stream and the current Squeek block. | 400 × 230 |
| Block banner | A full-width banner with copper sparks whenever a settled round completes Squeek blocks, with the SQK minted and the round number. | 1920 × 1080 |
| Chat calls | Viewers type `!run` or `!nap` in Twitch chat. When the next 10-minute round settles on Sepolia, the board stamps the result (work added > 0 = he ran) and keeps everyone's streak; the top three streaks are listed. | 400 × 330 |
| Ticker | A lower third with a live tally, a crawl of wheel and chain stats, and a clock counting down to the next round. | 1920 × 60 |

`overlay.html?w=all` shows every overlay laid out for a 1920 × 1080 scene.

### Link options (`overlay.html`)

| Param | Values | Default |
|---|---|---|
| `w` | `alert`, `gauge`, `block`, `poll`, `ticker`, `all` | `all` |
| `channel` | Twitch channel name (turns on chat calls) | none |
| `theme` | `broadcast`, `copper`, `daylight` | `broadcast` |
| `scale` | 0.3 – 3 | 1 |
| `sound` | `0` to mute | on |
| `nap` | `1` to also announce naps | off |
| `secs` | how long the run alert stays, 4 – 30 | 9 |
| `demo` | `1` simulates a running hamster, chat and blocks | off |

## How it uses Squeekchain

Everything is read from Squeekchain's public API (`https://api.squeekchain.com/v1`) by the streamer's own OBS:

- `/status` every 5 seconds: `measurement.coreStatus`, `rpmMilli`, `speedMmPerSecond`, `distanceMm`; `builder.projected.blockHeight` and `blockProgress`.
- `/settlements?range=24h` every 30 seconds: new entries in `recentSettlements` trigger block banners and settle chat calls (`workAdded`, `completedBlockCount`, `lastCompletedBlock`, `sqkMintedBaseUnits`, `settledThroughRound`); `summary` and `buckets` feed the ticker.

Twitch chat is read anonymously over Twitch's public IRC WebSocket (read-only `justinfan` login). Wheelcast never posts and never asks for a token. Prediction streaks are stored in the OBS browser's localStorage, per channel. Sounds are synthesised in the browser, so there are no audio files.

Squeektoshi's wheel time is never forced or encouraged; naps are part of the show.

## Files

- `index.html`: the settings page and program monitor
- `overlay.html`: all five overlays (one file, picked with `?w=`)
- `assets/`: the hamster and wheel artwork (original, made with Google Gemini)

## Author

Made by serkan ([@serkankels](https://x.com/serkankels) on X).

## License

MIT

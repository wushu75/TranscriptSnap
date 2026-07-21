# Testing TranscriptSnap

No build step and no test runner — load it unpacked and work through this list. Takes about ten minutes.

## Load it

1. Open `chrome://extensions`.
2. Toggle **Developer mode** on (top right).
3. Click **Load unpacked** → select the `extension/` folder.
4. Confirm the card shows **TranscriptSnap 1.0.0** with no red error banner.
5. Click **Service worker** on the card → the console should be silent.
6. Pin the extension to the toolbar so you can see the badge.

After editing any file: click the **reload icon** on the extension card, then **hard-reload the YouTube tab** (Cmd/Ctrl+Shift+R). Content scripts don't hot-swap.

## Core path — video with captions

Use any talk, tutorial or podcast that shows a "Show transcript" button under the description.

| # | Do this | Expect |
| --- | --- | --- |
| 1 | Open the video | A "Transcript" pill in the bottom-right corner |
| 2 | Click the pill | Panel opens, chip reads **Ready** |
| 3 | Click **Transcribe** | Chip → **Transcribing…**, thin progress bar animates, then chip → **Transcribed** and text appears |
| 4 | Read the text | Paragraphs, no `0:14` timestamps, no `[Music]`, no doubled lines |
| 5 | Click **Copy transcript** | Chip → **Copied** for ~1.6s, then back to **Transcribed**. Paste somewhere to verify |
| 6 | Click **Download .txt** | File saves, named after the video, with title + URL at the top |
| 7 | Click **Timestamps off** | Button reads **Timestamps on**, text switches to `[0:12] line` format instantly |
| 8 | Copy and download again | Both now include timestamps |
| 9 | Check the toolbar badge | Green ✓ |

## Error path — video without captions

Find a video with no "Show transcript" button (many music videos and small uploads).

- Click **Transcribe** → chip reads **Not available**, panel shows a plain-English explanation, no console exceptions.
- The Copy and Download buttons stay disabled.
- Badge shows a red `!`.

## Popup

1. Click the toolbar icon on a YouTube watch page → chip mirrors the page state.
2. **Transcribe this video** from the popup → panel opens on the page and fills in.
3. **Copy transcript** from the popup → clipboard works (this is the case where Chrome's clipboard API can fail and the fallback kicks in — check the paste).
4. **Timestamps** toggle in the popup and in the panel stay in sync after reopening.
5. Open the popup on a non-YouTube tab (e.g. google.com) → "Open a youtube.com/watch page to get started", Transcribe disabled.

## Single-page navigation

YouTube doesn't reload between videos, which is the most common source of stale-state bugs.

1. Transcribe video A.
2. Click a suggested video in the sidebar — don't reload.
3. Panel closes, launcher pill returns, state resets to **Ready**.
4. Transcribe video B → you get B's transcript, not A's.
5. Use the browser Back button → still resets cleanly.

## Persistence

1. Turn timestamps **on**, close the tab, open a new YouTube video.
2. Transcribe → timestamps are still on.
3. `chrome://extensions` → Service worker → Application → Storage: the only key is `showTimestamps`. **No transcript text anywhere.**

## Privacy verification

1. Open DevTools → **Network**, filter to `Fetch/XHR`, and clear it.
2. Click Transcribe.
3. Confirm the extension issued no requests. (YouTube's own requests will appear — check the Initiator column; nothing should originate from `chrome-extension://`.)
4. Also check the service worker's own Network tab: empty.

## Appearance and accessibility

- **Dark mode**: set your OS to dark → panel and popup follow, text stays readable.
- **Keyboard**: Tab into the panel — every control shows a teal focus ring; **Esc** closes the panel.
- **Reduced motion**: enable it in OS settings → the progress bar stops animating and shows a solid bar.
- **Zoom**: at 150% page zoom the panel stays on screen and buttons don't overlap.
- **Narrow window**: shrink to ~500px wide → panel caps at `100vw - 32px`.
- **Long videos**: try a 2-hour video → panel scrolls, no lag, download completes.

## Before shipping a release

- [ ] Every row above passes on a fresh Chrome profile.
- [ ] `manifest.json` version bumped.
- [ ] No `console.log` left in `content.js`, `popup.js`, `background.js`.
- [ ] Zip built from the *contents* of `extension/` (manifest at the zip root).
- [ ] Zip loads unpacked after unzipping, on a machine that never had the dev copy.

# TranscriptSnap

**A free, local-only YouTube transcript cleaner for Chrome.** Pull the transcript from the video you're watching, strip the timestamps and caption clutter, and copy or download clean text.

No backend. No account. No network requests — not one.

[![Chrome Web Store](https://img.shields.io/chrome-web-store/v/pehnikkgaikdmpdmodoemfcifboopbil?label=Chrome%20Web%20Store)](https://chromewebstore.google.com/detail/transcriptsnap/pehnikkgaikdmpdmodoemfcifboopbil?authuser=0&hl=en-GB)
[![Users](https://img.shields.io/chrome-web-store/users/pehnikkgaikdmpdmodoemfcifboopbil)](https://chromewebstore.google.com/detail/transcriptsnap/pehnikkgaikdmpdmodoemfcifboopbil)
[![Rating](https://img.shields.io/chrome-web-store/rating/pehnikkgaikdmpdmodoemfcifboopbil)](https://chromewebstore.google.com/detail/transcriptsnap/pehnikkgaikdmpdmodoemfcifboopbil)

⬇️ **[Install from the Chrome Web Store](https://chromewebstore.google.com/detail/transcriptsnap/pehnikkgaikdmpdmodoemfcifboopbil)**

🔗 **[Website](https://wushu75.github.io/TranscriptSnap/)** · **[Privacy policy](https://wushu75.github.io/TranscriptSnap/privacy.html)** · **[Report a bug](https://github.com/wushu75/TranscriptSnap/issues/new/choose)**

---

## What it does

YouTube's built-in transcript is a wall of two-second fragments, repeated lines, `[Music]` cues and timestamps. TranscriptSnap turns that into prose:

| Before | After |
| --- | --- |
| `0:12  so the first thing you want to do is`<br>`0:14  [Music]`<br>`0:15  so the first thing you want to do is`<br>`0:17  open up the settings panel and then` | So the first thing you want to do is open up the settings panel and then scroll down to where it says export. |

- **One click** on any `youtube.com/watch` page
- **Copy transcript** or **Download .txt**
- **Timestamps toggle** — off for reading, on for citing
- **Status chip** that tells you where you are: Ready → Transcribing → Transcribed → Copied, or a plain-English error

## Privacy

The extension reads the transcript YouTube **already renders in the page DOM**. That's the whole mechanism — which is why it needs no API key, no server and no connection. Open DevTools → Network while you use it; you'll see nothing.

The only thing written to storage is one boolean: whether timestamps are shown.

## Install

### From the Chrome Web Store (recommended)

**[Add TranscriptSnap to Chrome](https://chromewebstore.google.com/detail/transcriptsnap/pehnikkgaikdmpdmodoemfcifboopbil)** — one click, updates automatically.

### From a release

1. Download `TranscriptSnap.zip` from the [latest release](https://github.com/wushu75/TranscriptSnap/releases/latest) and unzip it.
2. Go to `chrome://extensions`.
3. Turn on **Developer mode** (top right).
4. Click **Load unpacked** and select the unzipped folder.
5. Open any YouTube video with captions.


## Known limits

- Videos with captions disabled by the uploader have no transcript to extract. The extension says so rather than failing silently.
- Live streams only expose a transcript once captions are finalised.
- TranscriptSnap depends on YouTube's DOM structure. If YouTube changes its transcript markup, extraction can break — [open an issue](https://github.com/wushu75/TranscriptSnap/issues/new/choose) and it gets patched.
- Multi-language videos use whichever caption track YouTube has selected.

## Contributing

Bug reports and small, focused pull requests are welcome. Two rules that won't bend:

1. **No network calls.** Not for fonts, not for analytics, not for "just one API".
2. **No new permissions** without a strong justification in the PR description.

## Licence

MIT.

Not affiliated with, endorsed by or sponsored by YouTube or Google.

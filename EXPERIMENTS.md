# Notes on Experimental branch
## 🎥 Playback Strategy

### HLS → Progressive Fallback

The player prioritizes **HLS streams** first, and falls back to **progressive playback** if needed.

1. Try loading media as HLS (`.m3u8`)

2. If HLS parsing or playback fails:

    - Fallback to progressive stream (e.g. `.mp4`, direct file)

3. Ensures maximum compatibility across sources

---
## 🌐 MIME Type Detection (Network streams)

Before passing media to ExoPlayer, the app attempts to **determine the correct MIME type dynamically**.

**Approach:**

1. Perform a request (HEAD )

2. Read `Content-Type` from response headers

3. Map it to appropriate ExoPlayer `MimeTypes`

4. Set MIME before creating MediaItem


**Why:**

- URLs may not have reliable extensions

- Some servers return misleading file names

- Prevents incorrect player behavior or playback failure


---

## 📲 Intent-Based Playback Support

The app supports launching playback via Android intents.

### Supported Intent

- `android.intent.action.VIEW`


### Expected Data

- Video URL passed via `-d`

- Optional headers passed via `--es headers`


---

## 🧪 Working Intent Example

Use this on termux to launch the player:

```bash
am start \
  -a android.intent.action.VIEW \
  -d "https://fast.vidplus.dev/file2/.../index.m3u8?host=rainorbit33.xyz" \
  -n dev.anilbeesetti.nextplayer.debug/dev.anilbeesetti.nextplayer.feature.player.PlayerActivity \
  --es headers "Referer=https://www.vidking.net/;Origin=https://www.vidking.net"
```

---

## 🧾 Header Injection

Custom headers are supported via intent extras.

**Format:**

```
Referer=https://example.com/;Origin=https://example.com
```

`;` is to separate headers.

---


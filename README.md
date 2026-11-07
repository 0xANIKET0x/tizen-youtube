# YouTube for Tizen

Ad-free YouTube on a Samsung TV with native Cobalt performance.

<img src="app/icon.png" width="96" align="right">

- Adverts and sponsor segments gone, running in the TV's own YouTube client
- Dedicated homescreen application
- Uses native Cobalt container for hardware-accelerated 4K/HDR/60fps playback
- Tizen 5.0 up to Tizen 10.0

---

## Tizen 9 & 10 Workaround (Issue #101 Fix)

On Samsung TVs running **Tizen 9.0 and 10.0**, Samsung modified the Web Runtime (WRT) process lifecycle:
- Background services configured with `on-boot="true"` or `auto-restart="true"` are no longer automatically awakened by the OS.
- When launching the standard Tube application, Samsung TV opens the native Cobalt container directly (`com.samsung.tv.cobalt-yt`). Because Cobalt runs in a restricted sandbox without WRT APIs, it cannot start the background service and gets stuck indefinitely on:
  ```text
  service: not answering yet (10s)...
  ```
- Tizen 9 and 10 require the service to be awakened via the Web Runtime `wrt:service` module (`new Service('tUb3Xq7Lm9.TubeService').start()`).

### The Fix: "YT service and launcher"

This fork includes **YT service and launcher** (`tube-launcher.wgt`), a lightweight Tizen WRT companion application that:
1. Awakens `tUb3Xq7Lm9.TubeService` via `wrt:service`.
2. As soon as the service start is acknowledged, displays **"You can open YT now"**.
3. Immediately transitions into YouTube (`tUb3Xq7Lm9.Tube`).
4. **Session-Aware Re-launch**: Detects live process contexts and session state in memory. Any repeat launch within the same session bypasses re-starting the service and launches YouTube directly every single time.

---

## Installation

Download the pre-compiled widgets from the [Releases](https://github.com/0xANIKET0x/tizen-youtube/releases/tag/v1.5.1-tizen10) page or the [`release/`](release/) directory in this repo.

### For Tizen 9.0 and 10.0 TVs (Recommended)

Install **both** `.wgt` packages on your TV:

1. **`tube-tizen-5.5.wgt`** (The main YouTube app & Node background service)
2. **`tube-launcher.wgt`** (**YT service and launcher**)

#### Installation Methods:

##### Option A: Installing via Apps2Samsung (Recommended GUI)
You can also install both `.wgt` files easily using **Apps2Samsung**:
1. Download `tube-tizen-5.5.wgt` and `tube-launcher.wgt` from [Releases](https://github.com/0xANIKET0x/tizen-youtube/releases/tag/v1.5.1-tizen10).
2. Open **Apps2Samsung** on your computer.
3. Sign both widgets using your Samsung Author / Partner Certificate.
4. Connect to your TV IP address and install both packages.

##### Option B: Installing via SDB (Command Line)
```bash
sdb connect <YOUR_TV_IP>
sdb install release/tube-tizen-5.5.wgt
sdb install release/tube-launcher.wgt
```

##### Option C: Installing via TizenBrew
Load both packages through the **Upload** tab in TizenBrew.

#### How to use:
- On your TV home screen, launch **YT service and launcher**.
- The service will start, display **"You can open YT now"**, and hand off to YouTube automatically.

---

### For Tizen 5.5 to 8.x TVs
Only the main widget is needed:
- Install `tube-tizen-5.5.wgt`.

### For Tizen 5.0 TVs
- Install `tube-tizen-5.0.wgt`.

---

## Building from Source

Requirements: **Node.js 20+**

```bash
git clone https://github.com/0xANIKET0x/tizen-youtube.git
cd tizen-youtube
npm install
node tools/package.js
```

All compiled `.wgt` packages will be generated inside the `release/` folder:
- `release/tube-tizen-5.5.wgt`
- `release/tube-launcher.wgt`
- `release/tube-tizen-5.0.wgt`

---

## Features

| Feature | Details |
| --- | --- |
| **Engine** | Samsung's own Cobalt container (identical to stock YouTube TV client) |
| **Ad blocking** | Native ad suppression with zero buffering jumps |
| **SponsorBlock** | Seamlessly integrated segment skipping |
| **DeArrow** | Clickbait titles and thumbnails swapped as feed arrives |
| **Return YouTube Dislike** | Real dislike counts displayed in player UI |
| **Video Quality** | Hardware-accelerated 4K, HDR, 60fps |
| **Tizen 9 & 10 Support** | Zero-lag background service bridge |

---

## License

Licensed under GPL-3.0. Upstream project by [SushyDev](https://github.com/SushyDev/tizen-youtube). Derived from [TizenTube](https://github.com/reisxd/TizenTube) and [youtube-webos](https://github.com/webosbrew/youtube-webos).


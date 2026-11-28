<div align="center">

# Yumoon

**All-in-One Streaming, Comics, Hi-Fi Music & Community App for Android**

![Version](https://img.shields.io/badge/version-2.1.0%20%C2%B7%20vc5-007AFF?style=flat-square)
![Platform](https://img.shields.io/badge/platform-Android%207%2B%20%7C%20Windows%20APK-34C759?style=flat-square)
![UI](https://img.shields.io/badge/design-iOS%20Dark%20%26%20White%20%C2%B7%20Liquid%20Glass-1C1C1E?style=flat-square)
![Stack](https://img.shields.io/badge/stack-Kotlin%20%C2%B7%20Jetpack%20Compose%20%C2%B7%20Media3%20%C2%B7%20Cloudflare%20D1-FF9500?style=flat-square)

Watch anime, donghua, movies, and Asian dramas, read manga and manhwa, listen to ad-free music with AI depth posters, and chat with the community in one clean, fast, iOS-inspired app.

</div>

---

## Highlights

### Root-to-Leaf iOS Dark & iOS White UI
- Built with **Haze** frosted blur, **Compose Cupertino** native controls (`CupertinoSwitch`, `CupertinoSegmentedControl`, `CupertinoActivityIndicator`), **Kyant0 Liquid Glass**, and continuous **Squircle** corner curvature across every card, poster, dialog, sheet, and menu.
- Authentic **iOS OLED Dark** (`#000000` / `#1C1C1E` / `#0A84FF`) and **iOS White** (`#F2F2F7` / `#FFFFFF` / `#007AFF`) verified for high readability and WCAG AA contrast.
- Full-screen iOS Settings with live search, left-edge swipe back, collapsing Large Title, and Taptic-style micro haptics.
- Fluid Apple-style push/pop parallax transitions, SF-Symbol-inspired bouncy tab icons, and GPU-accelerated spring physics.

### Stream
- Multi-source catalog covering anime, donghua, movies, Korean/Chinese/Japanese dramas, short dramas, and tokusatsu.
- Media3 ExoPlayer with quality switching, online subtitle search, offline batch downloads, and live **Nobar** watch parties over room codes.
- Multiple home layout presets (`Yumoon iOS`, Netflix, Viu, iQIYI, Loklok, Crunchyroll, WeTV, Viki, Youku).

### Comics
- Manga, manhwa, and manhua reader with vertical webtoon scroll, auto-scroll, chapter prefetching, and separated per-source reading history.
- Optional **Plus** edition with PIN and Biometric locked 18+ sources.

### Music
- Ad-free YouTube Music streaming with background playback, offline audio downloads, and frosted Haze synced lyrics.
- **Persistent iOS Floating Mini Player** docked right above the bottom navigation bar with swipe left/right to skip tracks, swipe up to open the full player, inline quick search, and queue drawer.
- **AI 3D Depth Poster Studio** powered by on-device ML Kit Subject Segmentation to generate layered lock-screen style music posters.
- 5-band Hi-Fi Equalizer with live curve graph, Slowed + Reverb, Nightcore, 8D Spatial, and **Music Room** Listen Together.

### Direct SD Card & Custom Folder Downloads
- **Zero internal staging** like Chrome and 1DM: when an SD Card or device folder is chosen via Android SAF, large MP4 videos, HLS `.m3u8` streams with in-RAM AES-128 decryption, comic chapters, and music stream directly to the chosen folder using 0 MB of internal app storage.
- Automatic HTTP `Range` resume on connection drops and zero silent fallback to internal storage.

### Social, Progression & Multitasking
- Global Public Chat and Private DMs with Haze frosted headers, swipe replies, double-tap reactions, photo/video sharing, custom sticker maker, and Voice Rooms.
- Server-verified EXP, levels, cultivation realms, daily streaks, missions, gacha, avatar borders, chat bubbles, and animated nameplates backed by Cloudflare Workers + D1.
- Floating resizable Mini-Yumoon window and adaptive Home Screen widgets.

---

## Builds & Editions

| Target | Format | ABIs | Version | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Android Clean** | `.apk` | `arm64-v8a`, `armeabi-v7a` | `2.1.0` · vc5 | Standard SFW edition, split per ABI for minimal size |
| **Android Plus 18+** | `.apk` | `arm64-v8a`, `armeabi-v7a` | `2.1.0` · vc5 | Includes PIN & Biometric locked 18+ stream and comic sources |
| **Windows PC APK** | `.apk` | `arm64-v8a`, `x86_64` | `2.1.0` · vc5 | Lightweight resizable APK for Windows WSA and emulators |

Download prebuilt APKs from **Releases** or run **Build Yumoon Release APKs** under **Actions**.

---

## Architecture

```text
Yumoon/
├── app/                            # Android Kotlin + Jetpack Compose application
│   ├── src/main/                   # Core Stream, Comic, Music, Social, Widgets, iOS Theme
│   ├── src/clean/                  # Clean SFW flavor stubs
│   ├── src/full/                   # Plus 18+ flavor scrapers & network rules
│   └── src/windows/                # Lightweight Windows PC APK flavor
├── web/                            # Official Yumoon Web Portal (Cloudflare Pages + D1 + GSAP/Motion/Lenis)
│   ├── _worker.js                  # Download gateway, permanent D1 counter & server-driven web config API
│   ├── wrangler.toml               # Cloudflare Pages D1 binding configuration
│   ├── ios-motion.js               # Lenis smooth scroll + GSAP ScrollTrigger + Motion springs
│   └── index.html / style.css / app.js
├── server/cloudflare-progression/  # Cloudflare Worker + D1 SQLite backend (Chat, Push, RPG, Rooms, Web Config)
└── .github/workflows/              # Automated multi-flavor Release APK CI
```

---

## Building & Server Setup

### 1. Android APKs
Requires **JDK 17** and **Android SDK 35**.

```bash
# Clean mobile release
./gradlew :app:assembleCleanRelease

# Plus 18+ mobile release
./gradlew :app:assembleFullRelease

# Windows PC APK release
./gradlew :app:assembleWindowsRelease
```

### 2. Web Portal Permanent D1 Download Counter & Remote Config
The `web/` portal runs on **Cloudflare Pages** with `_worker.js` connected to Cloudflare D1 (`yumoon_progression_db`).

1. In **Cloudflare Pages -> Project Settings -> Bindings**, attach your D1 database as binding name **`DB`** pointing to `yumoon_progression_db` (or set env var `PROGRESSION_API_URL` to your `yumoon-progression` Worker URL).
2. Add environment variable **`ADMIN_SECRET`** to enable live server updates via `POST /api/admin/web-config`.
3. Download counts are persisted permanently in D1 tables `apk_download_totals` and `apk_download_variants` and never drop even when older GitHub Releases are replaced.
4. All web copy (`strings.id`, `strings.en`), brand/announcement text (`brand`), and external links (`links.telegram`, `links.coffee`, `links.developer`, custom download URLs) can be changed live anytime from D1 `web_portal_config` or `POST /api/admin/web-config` without redeploying HTML/JS.

---

<div align="center">
Made with care by <strong>RapSyncFox</strong>
</div>

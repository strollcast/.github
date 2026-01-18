# Strollcast

Strollcast transforms ML research papers into audio podcasts. Listen to cutting-edge research while you walk, commute, or exercise.

## Mobile Apps

<p align="center">
  <img src="https://strollcast.com/screenshots/iphone-main.png" alt="iOS App" height="400" />
  &nbsp;&nbsp;&nbsp;
  <img src="https://strollcast.com/screenshots/android-main.png" alt="Android App" height="400" />
</p>

### iOS
- Native SwiftUI app
- Siri voice commands for hands-free control
- Zotero integration for syncing notes to your research library
- Available via TestFlight

### Android
- Native Kotlin/Jetpack Compose app
- Background playback with media controls
- Timestamped notes
- Available on [Google Play](https://play.google.com/store/apps/details?id=com.strollcast.app)

## Repositories

- **[director](https://github.com/strollcast/director)** - Astro website and Cloudflare Worker API for podcast generation
- **[StrollcastApp](https://github.com/strollcast/StrollcastApp)** - Native iOS and Android apps for listening and note-taking

## How It Works

1. Submit an arXiv paper URL
2. Claude generates a conversational podcast script
3. ElevenLabs TTS creates natural-sounding audio
4. Listen on the web or mobile apps

## Tech Stack

- **Frontend:** Astro SSR on Cloudflare Pages
- **API:** Cloudflare Workers + D1 + R2 + Queues
- **AI:** Anthropic Claude (scripts), ElevenLabs (TTS)
- **iOS:** SwiftUI with Zotero integration
- **Android:** Kotlin + Jetpack Compose + Media3

## Links

- **Website:** [strollcast.com](https://strollcast.com)
- **iOS Support:** [strollcast.com/support/ios](https://strollcast.com/support/ios)
- **Android Support:** [strollcast.com/support/android](https://strollcast.com/support/android)

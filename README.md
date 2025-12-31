# Strollcast

Strollcast transforms ML research papers into audio podcasts. Listen to cutting-edge research while you walk, commute, or exercise.

## Repositories

- **[director](https://github.com/mseritan/director)** - Astro website and Cloudflare Worker API for podcast generation
- **[StrollcastApp](https://github.com/mseritan/StrollcastApp)** - Native iOS app for listening and note-taking

## How It Works

1. Submit an arXiv paper URL
2. Claude generates a conversational podcast script
3. ElevenLabs or Inworld TTS creates natural-sounding audio
4. Listen on the web or iOS app

## Tech Stack

- **Frontend:** Astro SSR on Cloudflare Pages
- **API:** Cloudflare Workers + D1 + R2 + Queues
- **AI:** Anthropic Claude (scripts), ElevenLabs/Inworld (TTS)
- **iOS:** SwiftUI with Zotero integration

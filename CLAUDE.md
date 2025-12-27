# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Strollcast is a dual-platform application that transforms ML research papers into audio podcasts. The repository contains:
- **static-site/**: Astro website hosting episodes
- **static-site/modal/**: Serverless podcast generation (Modal + Cloudflare R2)
- **static-site/python/**: Local podcast generation tools (legacy, for preview)
- **StrollcastApp/**: Native iOS app for listening and note-taking

## Build & Run Commands

### Static Site (Astro)
```bash
cd static-site
npm install        # Install dependencies
npm run dev        # Start dev server at localhost:4321
npm run build      # Production build to dist/
```

### iOS App
```bash
cd StrollcastApp
open StrollcastApp.xcodeproj   # Open in Xcode
# Cmd+R to build and run
```

### Podcast Generation (Modal - Production)
```bash
cd static-site/modal

# Generate episode (runs on Modal, stores in Cloudflare R2)
modal run -m src.generator \
    --script-path ../public/<episode-folder>/script.md \
    --episode-name <episode-folder>

# Deploy the app (keeps it warm for faster runs)
modal deploy -m src.app
```

### Podcast Generation (Local - Preview Only)
```bash
cd static-site/python

# Preview with macOS TTS (free, instant, no API needed)
pixi run python generate.py ../public/<episode-folder> --preview
```

## Architecture

### Episode Data Flow
1. `public/<author>-<year>-<name>/script.md` - Markdown with `**ERIC:**` / `**MAYA:**` speaker tags
2. `modal/` - Modal function generates audio via ElevenLabs, normalizes to -16 LUFS, creates VTT
3. **Cloudflare R2 `strollcast-cache`** - Cached normalized audio segments (keyed by text hash)
4. **Cloudflare R2 `strollcast-output`** - Final episodes (`.m4a`) and transcripts (`.vtt`)
5. `public/api/episodes.json` - Episode metadata API consumed by iOS app

### iOS App Architecture
- **Services/**: Business logic (PodcastService, AudioPlayer, DownloadManager, ZoteroService)
- **Views/**: SwiftUI views for podcast list, player, notes, settings
- **Tab structure**: Podcasts → Played → Notes → Settings
- Zotero integration: Auto-adds papers to library, syncs notes as child items

### Key Conventions
- Episode folder naming: `<first-author>-<year>-<short-name>/`
- Audio file naming: `<folder-name>.m4a`
- Speaker tags in scripts must be bold: `**ERIC:**` and `**MAYA:**`
- Hosts are AI-generated voices, always introduced as virtual/AI hosts
- Sign-off: "Until next time, keep strolling" / "And may your gradients never explode"

## Adding a New Episode

1. Create folder: `static-site/public/<author>-<year>-<short-name>/`
2. Write `script.md` with speaker tags
3. Preview (optional): `cd python && pixi run python generate.py ../public/<folder> --preview`
4. Generate:
   ```bash
   cd static-site/modal
   modal run -m src.generator \
       --script-path ../public/<folder>/script.md \
       --episode-name <folder>
   ```
5. Add `README.md` with episode metadata
6. Update `src/pages/index.astro` episodes array
7. Update `public/api/episodes.json` (iOS app consumes this)

## Voice Configuration

- **Eric (male)**: ElevenLabs `gP8LZQ3GGokV0MP5JYjg`, macOS `Daniel`
- **Maya (female)**: ElevenLabs `21m00Tcm4TlvDq8ikWAM` (Rachel), macOS `Samantha`
- Model: `eleven_turbo_v2_5`
- Audio normalized to -16 LUFS (podcast standard)
- Cached audio stored in Cloudflare R2 `strollcast-cache` bucket (normalized, keyed by text+voice hash)

## Deployment

- **Static site**: Auto-deploys to GitHub Pages on push to main
- **iOS app**: GitHub Actions builds unsigned IPA on version tags (v*)
- **Podcast generation**: Modal serverless (deploy with `modal deploy -m src.app`)
- **Audio storage**: Cloudflare R2 (`strollcast-cache` for segments, `strollcast-output` for episodes)

## Modal Secrets

The Modal app requires two secrets configured at https://modal.com/secrets:

| Secret | Keys |
|--------|------|
| `elevenlabs` | `ELEVENLABS_API_KEY` |
| `cloudflare-r2` | `R2_ENDPOINT`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY` |

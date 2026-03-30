# yt-dlp Downloader (OpenClaw)

A standalone OpenClaw-compatible version of the yt-dlp downloader skill. It downloads videos from YouTube, Bilibili, Twitter/X, TikTok/Douyin, Vimeo, Twitch, and more via yt-dlp, with options for audio-only (MP3), subtitles, and quality selection.

## Features
- Best-quality video download by default
- Audio extraction (MP3)
- Subtitles (all languages)
- Quality filters (e.g., 720p/1080p)
- Playlist support and format listing

## Directory
```
openclaw/yt-dlp-downloader/
├── SKILL.md          # OpenClaw skill instructions + gating/installer metadata
├── skill.yaml        # OpenClaw manifest (name/version/permissions/entryPoint)
└── scripts/
    └── download.sh   # Optional helper wrapper for advanced usage
```

## Prerequisites
- yt-dlp (required)
- ffmpeg (recommended for audio extraction and format merging)

This skill declares installer metadata so the OpenClaw UI/CLI can assist with installing `yt-dlp` and (on macOS) `ffmpeg`.

## Install (local)
Validate and install the skill into your OpenClaw instance:


1) Install
```
openclaw skills install ./yt-dlp-downloader
```

2) Verify
```
openclaw skills list
```

### Optional: Enable via config (JSON5)
`~/.openclaw/openclaw.json`:
```json5
{
  skills: {
    entries: {
      "yt-dlp-downloader": { enabled: true }
    }
  }
}
```

## Usage
Ask your agent to download or extract media by providing a URL. Examples:

- Best quality
```
Download this video https://www.youtube.com/watch?v=xxx
```

- Audio-only (MP3)
```
Extract audio from https://www.bilibili.com/video/xxx
```

- With subtitles
```
Download with subtitles https://twitter.com/xxx/status/123
```

- Specific quality
```
Download 720p https://www.tiktok.com/@user/video/xxx
```

Behind the scenes, the agent will construct a safe `yt-dlp` command using the default download directory from config (see below).

## Configuration
The skill accepts an optional `download_path` config key (default `~/Downloads/yt-dlp`). You can override it via OpenClaw config or at request time by explicitly specifying a safe output directory.

## Uninstall / Update
- Uninstall: `openclaw skills uninstall yt-dlp-downloader`
- Update (local path): re-run `openclaw skills install ./openclaw/yt-dlp-downloader`

## Troubleshooting
- YouTube 403 → Use cookies: the skill adds `--cookies-from-browser chrome` for YouTube when appropriate.
- `ffmpeg` missing when extracting audio → install ffmpeg (Homebrew on macOS or your distro package manager on Linux).
- Format not found → list first with `yt-dlp -F URL`, then choose a specific format.

## Security Notes
- Only http/https URLs are accepted; commands are constructed defensively to avoid arbitrary shell execution.
- Request minimal permissions; this skill uses `shell`, `filesystem`, and `network`.

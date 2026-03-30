---
name: yt-dlp-download-skill
description: Download videos from YouTube, Bilibili, Twitter/X, TikTok/Douyin and more using yt-dlp. Use when the user provides a video URL to download video, extract MP3 audio, fetch subtitles, or choose quality (720p/1080p/best).
metadata:
  openclaw:
    emoji: "⬇️"
    os: ["darwin", "linux", "win32"]
    requires:
      bins: ["yt-dlp"]
      anyBins: ["ffmpeg"]
    install:
      - id: uv-yt-dlp
        kind: uv
        package: "yt-dlp"
        bins: ["yt-dlp"]
        label: "Install yt-dlp (uv/pipx)"
      - id: brew-ffmpeg
        kind: brew
        formula: "ffmpeg"
        bins: ["ffmpeg"]
        os: ["darwin"]
        label: "Install ffmpeg (brew)"
---

# yt-dlp Video Downloader (OpenClaw)

Use yt-dlp to download videos from thousands of sites.

## When to use
- User shares a video URL and asks to download
- User requests audio-only (MP3) extraction
- User needs subtitles or specific quality

## Safety
- Never execute arbitrary shell built from untrusted text; always validate URL scheme (http/https only)
- Prefer the default download path under the user Downloads directory unless a safe path is explicitly provided

## Quick checks
- Ensure `yt-dlp` exists on PATH; if missing, use installer metadata to guide installation
- For MP3 extraction, `ffmpeg` is recommended; proceed with a warning if unavailable

## Common commands

- Best available quality
```bash
yt-dlp -P "~/Downloads/yt-dlp" "VIDEO_URL"
```

- YouTube with cookies (avoid 403)
```bash
yt-dlp -P "~/Downloads/yt-dlp" --cookies-from-browser chrome "YOUTUBE_URL"
```

- Extract audio (MP3)
```bash
yt-dlp -P "~/Downloads/yt-dlp" -x --audio-format mp3 "VIDEO_URL"
```

- With subtitles (all languages)
```bash
yt-dlp -P "~/Downloads/yt-dlp" --write-subs --sub-langs all "VIDEO_URL"
```

- Specific quality (720p / 1080p)
```bash
yt-dlp -P "~/Downloads/yt-dlp" -f "bestvideo[height<=720]+bestaudio/best[height<=720]" "VIDEO_URL"
```
```bash
yt-dlp -P "~/Downloads/yt-dlp" -f "bestvideo[height<=1080]+bestaudio/best[height<=1080]" "VIDEO_URL"
```

- List formats, then pick one
```bash
yt-dlp -F "VIDEO_URL"
yt-dlp -P "~/Downloads/yt-dlp" -f FORMAT_ID "VIDEO_URL"
```

## Workflow
1. Detect platform (YouTube often needs `--cookies-from-browser chrome`)
2. Ask for intent if unclear (video vs audio, subs, quality, output directory)
3. Build a safe command (http/https URL, sanitize output path)
4. Execute using the system exec tool
5. Report saved path and any errors; suggest `-F` on format errors

## Troubleshooting
- 403 on YouTube → add `--cookies-from-browser chrome`
- Missing ffmpeg for audio → warn and suggest installing ffmpeg
- Interrupted downloads → yt-dlp auto-resumes on retry

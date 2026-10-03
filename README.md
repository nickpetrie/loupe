# Loupe

Pick a 5-second stretch of video and export an MP4 or GIF that already meets the spec. One self-contained `index.html`, no build, nothing uploaded.

**MP4 (default):** H.264 via WebCodecs, 30 fps, 440×248 or 880×496 (2×, shown at 440×248), Compact / Balanced / High quality. Written by a small inline muxer with the index first (fast start), so browsers begin playing before the download finishes. The clip is captured during one real-time playthrough, so export takes about 5 s. Each file is read back and checked (dimensions, 5.00 s, 30 fps, under 5 MB, fast start, decodes in this browser) before download is offered. Embed it like a GIF:

```html
<video src="loupe-880x496-30fps.mp4" width="440" height="248" autoplay loop muted playsinline></video>
```

**GIF:** unchanged. 20 fps, then 12 fps, then stepped compression until it fits under 5 MB.

MP4 needs a browser with an H.264 WebCodecs encoder (current Chrome, Edge, Safari). Elsewhere the page falls back to GIF.

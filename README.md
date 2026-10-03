# Loupe

Turn a 5-second stretch of video, or a set of photos, into a looping MP4 or GIF that already meets the spec. One self-contained `index.html`: no build, no server, nothing uploaded.

## Modes

**Video clip.** Pick 5 s, frame it (locked to 440:248), add an optional label, export.
- MP4 (default where supported): H.264, 30 fps, 440×248 or 880×496 (2×), Compact / Balanced / High. Captured in one real-time playthrough, so export takes about 5 s.
- GIF: 20 fps, then 12 fps, then stepped compression until it fits under 5 MB.

**Photo slideshow.** Add up to 30 photos, give each a label, reorder, choose seconds per photo and cut or crossfade. Exports a seamless looping MP4 (the last photo fades back into the first). Photos are cropped to fill 440:248 from the centre.

**Labels.** Credit or location text burned into the frames, in any corner. The on-screen previews use the same geometry as the export.

**Recent downloads.** Each file you download is kept in this browser (IndexedDB) so it can be downloaded again: last 12, up to 60 MB. Nothing leaves the device.

Every export is read back and checked (dimensions, duration, frame rate, under 5 MB, index-first MP4, decodes in this browser) before download is offered.

## Embedding the MP4

```html
<video src="loupe-clip-880x496-….mp4" width="440" height="248" autoplay loop muted playsinline></video>
```

## Browser support

MP4 needs an H.264 WebCodecs encoder: current Chrome, Edge, Safari. Elsewhere the clip mode falls back to GIF and the slideshow is unavailable.

## Privacy and security

- No uploads, no accounts, no analytics. The page's Content-Security-Policy blocks every network request except Google Fonts.
- Exports are re-encoded from pixels, so photo EXIF (GPS, camera, timestamps) and source-video metadata are not carried over.
- Recent downloads live in the browser until removed. On shared machines, use **Clear all**.

# Loupe

Turn a 5-second stretch of video, or a set of photos, into a looping MP4 or GIF that already meets the spec. One self-contained `index.html`: no build, no server, nothing uploaded.

## Modes

**Video clip.** Pick 5 s, frame it (locked to 440:248), add an optional label, export.
- **Export MP4** (main button): H.264, 30 fps, constant quality, so calm footage comes out tiny and busy footage gets the bits it needs. Made at 880×496 (2×) when the framed area has the detail for it, otherwise 440×248. Captured in one real-time playthrough, so export takes about 5 s.
- **GIF** (secondary button, for places that only take GIF): 440×248, 20 fps, then 12 fps, then stepped compression until it fits under 5 MB.
- Shortcuts: Space play/pause, S start the clip at the playhead, P preview.

**Photo slideshow.** Add up to 30 photos, give each a label, reorder, choose seconds per photo and cut or crossfade. Exports a seamless looping MP4 (the last photo fades back into the first). Click a photo to set its crop: click or drag to choose the focal point, zoom up to 300% to frame a detail. Export re-reads each original at full resolution, so zoomed crops stay sharp.

**Labels.** Credit or location text burned into the frames, in any corner, for clips (MP4 and GIF) and slideshows. The on-screen previews use the same geometry as the export.

There are no quality or size settings: every export is made as small as it can be at a consistently clean quality, then checked. If a very busy clip would go over 5 MB, quality is eased a step automatically.

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

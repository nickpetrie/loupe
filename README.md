# Loupe

Turn a 5-second stretch of video, or a set of photos, into a looping MP4 or GIF that already meets the spec. One self-contained `index.html`: no build, no server, nothing uploaded.

## Orientation

Landscape (440×248) or portrait (248×440), switchable next to the framing controls and in the slideshow settings. Vertical videos switch to portrait on their own. Framing, photo crops, labels, previews, spec checks and filenames all follow the choice; MP4 doubles it (880×496 or 496×880) when the source has the detail.

## Modes

**Video clip.** Pick 5 s, frame it (locked to 440:248), add an optional label, export.
Pick **GIF**, **WebP** or **MP4** next to the Export button. GIF is the default because the website only takes GIF for now; the last choice is remembered per browser.

- **MP4**: H.264, 30 fps, constant quality, so calm footage comes out tiny and busy footage gets the bits it needs. Made at 880×496 (2×) when the framed area has the detail for it, otherwise 440×248. Captured in one real-time playthrough, so export takes about 5 s.
- **WebP**: animated WebP, which shows anywhere a GIF does in current browsers (an ordinary `<img>`), in full colour. Slideshows store each photo losslessly at 2× when that fits under 5 MB, with lightly compressed crossfade frames, stepping quality down only when needed; clips are 20 fps. Encoded by the browser: Chrome, Edge and Firefox can create WebP, Safari can only show it, so the option is disabled there.
- **GIF**: 440×248, 20 fps, then 12 fps, then stepped compression until it fits under 5 MB.
- Shortcuts: Space play/pause, S start the clip at the playhead, P preview.

**Photo slideshow.** Add up to 30 photos, give each a label, reorder, choose seconds per photo and cut or crossfade. Exports a seamless loop (the last photo fades back into the first) as GIF or MP4. As GIF, each photo is a single frame held for its full time, so stills are cheap; crossfades add three frames each. Slideshow GIFs are made at 2× (880×496, or 496×880 in portrait) when that fits under 5 MB, so they stay sharp on high-density screens; show them at 440×248. Otherwise it tries, in order: 1× with the chosen transition, 2× with cuts, 1× with cuts, then compresses in steps. A quick estimate skips 2× attempts that clearly won't fit. Click a photo to set its crop: click or drag to choose the focal point, zoom up to 300% to frame a detail. Export re-reads each original at full resolution, so zoomed crops stay sharp.

**Labels.** Credit or location text burned into the frames, in any corner, for clips (MP4 and GIF) and slideshows. The on-screen previews use the same geometry as the export.

There are no quality or size settings: every export is made as small as it can be at a consistently clean quality, then checked. If a very busy clip would go over 5 MB, quality is eased a step automatically.

**Recent downloads.** Each file you download is kept in this browser (IndexedDB) so it can be downloaded again: last 12, up to 60 MB. Nothing leaves the device.

Every export is read back and checked (dimensions, duration, frame rate, under 5 MB, index-first MP4, decodes in this browser) before download is offered.

## Max quality

A **Max quality** checkbox beside the format switch exports each format at its best and ignores the 5 MB limit (the size is still shown, flagged when it's over 5 MB). WebP: every frame lossless, crossfades included. GIF: 2×, 20 fps, best palette, no frame dropping or extra compression (still 256 colours a frame; GIF can't be lossless). MP4: near-lossless H.264 (QP 14; browsers can't encode truly lossless MP4). Remembered per browser.

## Embedding the MP4

```html
<video src="loupe-clip-880x496-….mp4" width="440" height="248" autoplay loop muted playsinline></video>
```

## Browser support

MP4 needs an H.264 WebCodecs encoder: current Chrome, Edge, Safari. GIF works everywhere, for clips and slideshows.

## Privacy and security

- No uploads, no accounts, no analytics. The page's Content-Security-Policy blocks every network request except Google Fonts.
- Exports are re-encoded from pixels, so photo EXIF (GPS, camera, timestamps) and source-video metadata are not carried over.
- Recent downloads live in the browser until removed. On shared machines, use **Clear all**.

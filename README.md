# Loupe

Turn a few seconds of video, or a set of photos, into a looping MP4 for homepage and category slabs, already within the slab spec (10 s, 2 MB). GIF and WebP are still there. One self-contained `index.html` (plus the logo files in `assets/`): no build, no server, nothing uploaded.

## The slab spec

- A short looping MP4 can replace a GIF on homepage and category page slabs. Upload it to the media library, then choose it with "select video" in the post editor's Featured video section.
- At most 10 s and 2 MB. The CMS warns on videos right at 10 s, so Loupe's longest clip is 9.9 s; 2 MB is read as 2,000,000 bytes, the stricter reading.
- The featured image is still required: it's shown instead of the video for reduced motion, data saver and slow connections, in the article, in feeds, Apple News, MSN, social and (for now) the app.
- Plays automatically, silent, looped, only while on screen, no controls.

## Orientation

Landscape (440×248) or portrait (248×440), switchable next to the framing controls and in the slideshow settings. Vertical videos switch to portrait on their own. Framing, photo crops, labels, previews, spec checks and filenames all follow the choice; MP4 doubles it (880×496 or 496×880) when the source has the detail.

## Modes

**Video clip.** Pick a start point and a length (3, 5, 8 or 9.9 s for MP4; GIF and WebP are always 5 s), frame it, add an optional label, export. A video shorter than the chosen length is used whole.
Pick **MP4**, **GIF** or **WebP** next to the Export button. MP4 is the default; the last choice is remembered per browser.

- **MP4**: H.264 High profile (level 3.1), 30 fps, constant quality (QP 23), so calm footage comes out small and busy footage gets the bits it needs. Made at 880×496 (2×) when the framed area has the detail for it, otherwise 440×248. One keyframe at the start (loops only ever jump back there), fast-start index, no audio track, tagged BT.709 (`colr`) so Safari and Windows don't shift the colours. If a busy stretch lands over 2 MB it's encoded again with the quality eased by what the overshoot needs; past QP 32 it drops to 1×, which needs about half the bytes for the same look. Captured in one real-time playthrough, so export takes about as long as the clip (longer if it has to be redone).
- **The source, checked first**: before capturing, Loupe plays a moment of the clip to read its frame rate and keeps it (24, 25 or 30 fps; 50 and 60 become 25 and 30), because turning 25 fps into 30 repeats every fifth frame and motion judders. It also checks for interlacing (broadcast 1080i/576i footage, where each frame holds two moments on alternate lines and anything moving gets comb-tooth edges). It reads the decoded brightness plane and looks for a one-line zig-zag that isn't there two lines apart. Interlaced sources are drawn from one field per frame: no combs, no ghosting, and a 1080i field still has more lines than the 496 the output needs. GIF and WebP get the same deinterlacing.
- **WebP**: animated WebP, which shows anywhere a GIF does in current browsers (an ordinary `<img>`), in full colour. Slideshows store each photo losslessly at 2× when that fits under 5 MB, with lightly compressed crossfade frames, stepping quality down only when needed; clips are 20 fps. Encoded by the browser: Chrome, Edge and Firefox can create WebP, Safari can only show it, so the option is disabled there.
- **GIF**: 440×248, 20 fps, then 12 fps, then stepped compression until it fits under 5 MB.
- Shortcuts: Space play/pause, S start the clip at the playhead, P preview.

**Photo slideshow.** Add up to 30 photos, give each a label, reorder, choose seconds per photo and cut or crossfade. Exports a seamless loop (the last photo fades back into the first) as MP4, GIF or WebP. As MP4 the loop must fit 9.9 s, so each photo's time shrinks to fit (down to 1 s, so up to 9 photos). As GIF, each photo is a single frame held for its full time, so stills are cheap; crossfades add three frames each. Slideshow GIFs are made at 2× (880×496, or 496×880 in portrait) when that fits under 5 MB, so they stay sharp on high-density screens; show them at 440×248. Otherwise it tries, in order: 1× with the chosen transition, 2× with cuts, 1× with cuts, then compresses in steps. A quick estimate skips 2× attempts that clearly won't fit. Click a photo to set its crop: click or drag to choose the focal point, zoom up to 300% to frame a detail. Export re-reads each original at full resolution, so zoomed crops stay sharp.

**Labels.** Credit or location text burned into the frames, in any corner, for clips (MP4 and GIF) and slideshows. The on-screen previews use the same geometry as the export.

There are no quality or size settings: every export is made as small as it can be at a consistently clean quality, then checked. If a very busy clip would go over the limit (2 MB for MP4, 5 MB for GIF and WebP), quality is eased automatically.

**Recent downloads.** Each file you download is kept in this browser (IndexedDB) so it can be downloaded again: last 12, up to 60 MB. Nothing leaves the device.

Every export is read back and checked (dimensions, duration, frame rate, under 2 MB for MP4 or 5 MB for GIF and WebP, index-first MP4, decodes in this browser) before download is offered.

## Max quality

A **Max quality** checkbox beside the format switch exports each format at its best and ignores the size limit (the size is still shown, flagged when it's over). A max-quality MP4 may be too big for a slab. WebP: every frame lossless, crossfades included. GIF: 2×, 20 fps, best palette, no frame dropping or extra compression (still 256 colours a frame; GIF can't be lossless). MP4: near-lossless H.264 (QP 14; browsers can't encode truly lossless MP4). Remembered per browser.

## Embedding the MP4

```html
<video src="loupe-clip-880x496-….mp4" width="440" height="248" autoplay loop muted playsinline></video>
```

## Design

Styled to The i Paper brand: Freeman for the title, Public Sans for text, DM Mono for labels; white page (dark mode #242629); Brand Red #EE3424 as the only accent. The logo files in `assets/` are used as supplied: the wordmark in light mode, the i mark alone in dark mode. Labels burned into exports use Public Sans on a square Neutral 950 box.

## Browser support

MP4 needs an H.264 WebCodecs encoder: current Chrome, Edge, Safari. GIF works everywhere, for clips and slideshows.

## Version

The small tag beside the name (for example "v14 · Smooth motion") is the release number and the name of its headline change. It lives in one place, the `data-version` and `data-name` attributes on `#versionTag` in `index.html`: bump both with every release. On load the page fetches its live copy without the cache and, if that's newer, the tag turns into "v14 · v15 available, tap to reload".

## Privacy and security

- No uploads, no accounts, no analytics. The page's Content-Security-Policy blocks every network request except Google Fonts.
- Exports are re-encoded from pixels, so photo EXIF (GPS, camera, timestamps) and source-video metadata are not carried over.
- Recent downloads live in the browser until removed. On shared machines, use **Clear all**.

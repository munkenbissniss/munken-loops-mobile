# Munken Loops Mobile

A simplified, phone-friendly version of [Munken Loops](https://github.com/munkenbissniss/munken-loops). Pick a rhythmic track and some seamless video loops, choose a mood, and the phone makes a soft, beat-synchronised video mix. Everything runs in the browser on the phone itself. Nothing is uploaded.

**Open it:** https://munkenbissniss.github.io/munken-loops-mobile/

## How to use

1. **Choose track** (mp3 or similar, rhythmic music works best).
2. **Choose videos.** Pick seamless loops from your gallery or files. Up to 24 are used, which keeps phones happy.
3. **Style:** pick a mood (**Recommended**, Ambient, Chill, Groove), how long each clip stays (Slow, Normal, Fast), the crossfade, and how many loops share each part of the song (2 or 3). *More options* has picture size, **frame rate (24, 30 or 60)**, orientation (portrait for vertical video), slow motion, breathing zoom, optional **smarter matching with AI (CLIP)** with an optional text theme, and a seed.
4. Press **Make video**. Progress, pause and stop are on the export card. When it is done, press **Download** or **Share / save**.

## What it does

The track is analysed for tempo, beats, song sections, breaks, drops and kick drums. Your loops are analysed for colour, texture and motion. Each part of the song gets 2-3 similar loops that alternate slowly (A-B-A-B), long shots on the bar lines, soft crossfades, a slow breathing zoom and a gentle colour tint that changes between parts. The video is recorded in real time from an offscreen canvas together with the music, and MP4 files are rewritten so they have a correct duration and can be seeked.

## Good to know on a phone

- **Real time:** a 4-minute track takes about 4 minutes. Keep the page open and the screen on. The app asks the phone to stay awake, and if you switch app it pauses and waits for you.
- **Memory:** iPhones close a page that uses too much memory, and the page then reloads by itself. To avoid that, the app uses at most three videos at once, releases the ones it no longer needs, stores the recording on disk (IndexedDB) instead of in memory, and defaults to 480p at 24 fps on iPhone. If the page still reloads, use a shorter track, fewer or smaller loops (1080p loops are much lighter than 4K), or 480p. After a reload the app tells you that it happened.
- **Screen:** the screen must stay on. The app uses Wake Lock where the browser supports it, and otherwise plays a tiny invisible video, which also keeps an iPhone awake. If the screen still locks, switch off Auto-Lock in iPhone Settings -> Display & Brightness while you make a video.
- **Format:** Safari (iPhone) and Chrome (Android) save MP4. A browser that can only save WebM shows a warning.
- **AI matching (CLIP) is on by default.** It downloads about 90 MB the first time (about 150 MB with a theme; use Wi-Fi) and looks at one frame per loop. It is heavy for a phone. To keep memory low it runs in a separate worker that is closed afterwards, and the loops and the AI are handled *before* the music is decoded. If the AI cannot start, the app carries on with standard matching and says so on the export card. If the page reloads on its own while the AI runs, the phone ran out of memory: the app tells you after the reload, and you should switch AI matching off (More options) or use fewer loops.
- **Fewer settings than the desktop version:** layers and the full advanced settings are in the [desktop version](https://github.com/munkenbissniss/munken-loops), which also has a command-line tool.
- Works best in the latest Chrome (Android) or Safari (iOS 15 or newer). On an iPhone every browser, including Chrome, uses Safari's engine, so Safari itself is the safest choice.
- If loops cannot be read (some iPhone videos are slow to open), they are skipped after a timeout and the rest are used.

## Run it yourself

It is a single static file, so any static host works. To use GitHub Pages: push this repo, then **Settings -> Pages -> Deploy from a branch -> `main` / `(root)`**.

To try it locally: `python3 -m http.server` in this folder, then open http://localhost:8000 (on a phone, use your computer's address on the same Wi-Fi; note that some phone features only work on https).

## License

MIT

## Recommended, and the new defaults

- **Recommended** analyses the music (tempo, beat steadiness, energy, drops, kick drums) and your loops (how much they move, how alike they look) and sets the mood, clip length, crossfade, loops per section and effects. A short explanation is shown under the mood buttons. Tap Pace, Crossfade or Loops per part afterwards to override that single setting.
- **Defaults:** 60 fps, AI matching (CLIP) on, and the brightness pulse dips dark. Choose 30 or 24 fps under *More options* if the phone struggles.

## If videos stand still or analysis does not start (iPhone)

An iPhone only plays videos from a web page when the page is visible and has been allowed by a tap, and **Low Power Mode blocks video playback completely**. The app therefore keeps its video elements in view, unlocks them when you tap *Make video* (or *Recommended*), starts reading each loop immediately, skips a loop that does not open in time, and stops with a clear message if none can be read. While making a video it watches for pictures that stop moving: it reloads that loop, and if the phone keeps refusing it pauses and asks you to turn off Low Power Mode and tap *Resume*.

## Sound quality

If your music is an MP3, the finished MP4 gets the **original MP3 audio** (no re-encoding, no tempo drift) instead of the live recording, and the picture is aligned and, if needed, stretched by a few percent so it ends exactly with the music. Other audio formats keep the recorded sound.

## Memory and battery

For an MP3 the app keeps only a small mono copy of the music in memory (about an eighth of the size of the decoded file) because the finished video gets the original MP3 as its sound. The recorded fallback audio uses a lower bit rate in that case. Picture quality, frame rate and the planner are unchanged.

## Upload straight to Vimeo (optional)

Open **Upload to Vimeo (optional)** in the app, paste a personal access token and choose who can watch. Create the token at developer.vimeo.com (Create an app, then Generate an access token with the scopes `upload`, `edit` and `private`). Optionally add a showcase number to put every upload in that showcase. When a video is finished, press **Upload to Vimeo**. The file goes from your browser straight to Vimeo (resumable upload in 16 MB parts, with retries); there is no server in between. The token is stored only in your browser. Your Vimeo plan's upload quota applies. Tested against a simulated Vimeo server; if your browser blocks the direct calls, download the video and upload it on vimeo.com instead.

## Storage on the phone

Videos are written to the browser's private storage while they are made, and kept there until you press **Remove** on them (download first). If the page is closed or reloaded in the middle of a run, the leftover files are deleted automatically the next time the page opens. Under **Storage on this phone** you can see how much space the page uses, delete everything it stored, or delete the downloaded AI model (it is cached in the browser and is only downloaded again if you delete it or clear Chrome's cached files).

## How Recommended chooses the mood (browser apps)

The mood now comes from the **music only**: how much is happening in it (rhythmic activity), how loud and bright it is, and the tempo. Your loops no longer decide the mood; they only fine-tune the typical shot length (about +/-8 %) and the slow motion amount. Calm, quiet music with little rhythm gets the slowest moods, busy bright loud music gets Groove.


## Smoother mixes

- **Adaptive transitions** (web app setting): each fade is made longer and softer between clips that look different or differ in brightness, and shorter between clips that are alike, with a gentler fade curve. On average the fades keep the length you chose.
- **Smooth flow between clips** (web app setting): the next clip is chosen so that it looks and feels like the one before it (colour, brightness, texture, motion), also across song sections. Every loop is still used. Both settings are 0 to 1; 0 switches them off. The mobile app uses the default values (0.6 and 0.5).

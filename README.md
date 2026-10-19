# Munken Loops Mobile

A simplified, phone-friendly version of [Munken Loops](https://github.com/munkenbissniss/munken-loops). Pick a rhythmic track and some seamless video loops, choose a mood, and the phone makes a soft, beat-synchronised video mix. Everything runs in the browser on the phone itself. Nothing is uploaded.

**Open it:** https://munkenbissniss.github.io/munken-loops-mobile/

## How to use

1. **Choose track** (mp3 or similar, rhythmic music works best).
2. **Choose videos.** Pick seamless loops from your gallery or files. Up to 24 are used, which keeps phones happy.
3. **Style:** pick a mood (Ambient, Chill, Groove), how long each clip stays (Slow, Normal, Fast), the crossfade, and how many loops share each part of the song (2 or 3). *More options* has picture size, orientation (portrait for vertical video), slow motion, breathing zoom, optional **smarter matching with AI (CLIP)** with an optional text theme, and a seed.
4. Press **Make video**. Progress, pause and stop are on the export card. When it is done, press **Download** or **Share / save**.

## What it does

The track is analysed for tempo, beats, song sections, breaks, drops and kick drums. Your loops are analysed for colour, texture and motion. Each part of the song gets 2-3 similar loops that alternate slowly (A-B-A-B), long shots on the bar lines, soft crossfades, a slow breathing zoom and a gentle colour tint that changes between parts. The video is recorded in real time from an offscreen canvas together with the music, and MP4 files are rewritten so they have a correct duration and can be seeked.

## Good to know on a phone

- **Real time:** a 4-minute track takes about 4 minutes. Keep the page open and the screen on. The app asks the phone to stay awake, and if you switch app it pauses and waits for you.
- **Memory:** on iPhone the finished file is kept in memory until you download it, so very long tracks at high quality can be too much. Start with 720p and tracks of a few minutes.
- **Format:** Safari (iPhone) and Chrome (Android) save MP4. A browser that can only save WebM shows a warning.
- **AI matching (CLIP) is optional and off by default.** It downloads about 90 MB the first time (about 150 MB with a theme; use Wi-Fi), looks at one frame per loop, and is released again before the video is recorded to save memory. If the phone cannot run it, the app carries on with standard matching and says so on the export card.
- **Fewer settings than the desktop version:** layers and the full advanced settings are in the [desktop version](https://github.com/munkenbissniss/munken-loops), which also has a command-line tool.
- Works best in the latest Chrome (Android) or Safari (iOS 15 or newer). On an iPhone every browser, including Chrome, uses Safari's engine, so Safari itself is the safest choice.
- If loops cannot be read (some iPhone videos are slow to open), they are skipped after a timeout and the rest are used.

## Run it yourself

It is a single static file, so any static host works. To use GitHub Pages: push this repo, then **Settings -> Pages -> Deploy from a branch -> `main` / `(root)`**.

To try it locally: `python3 -m http.server` in this folder, then open http://localhost:8000 (on a phone, use your computer's address on the same Wi-Fi; note that some phone features only work on https).

## License

MIT

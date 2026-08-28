# Subtitles (generated with [voxaline](https://github.com/scottvr/voxaline))

Word-timed `.ass` subtitles for the lyric video, produced by **voxaline** — a
tool that force-aligns known lyrics to the vocal stem and renders characterful
ASS typography (the display / spoken / effect split: `F-S-C-K` shows as four
giant quadrant letters while being aligned against "eff ess see kay").

## Files

| file | what it is |
|---|---|
| `lyrics.yaml` | voxaline input: per-line `display` / `spoken` text |
| `full_lyrics-phonetic.md` | the phonetic (spoken) lyric source that fed `lyrics.yaml` |
| `effects.yaml` | typography sidecar: styles + per-line effects + timing overrides |
| `timings.json` | word timings (hand-repaired where the aligner collapsed fast passages) |
| `alignment.raw.json` | the untouched stable-ts alignment output (reuse without re-running the model) |
| `lyrics.ass` | the generated subtitle file burned into the video |

## Regenerating

With voxaline checked out and this folder's files:

```bash
python ass.py --lyrics subtitles/lyrics.yaml \
              --effects subtitles/effects.yaml \
              --timings subtitles/timings.json \
              --out subtitles/lyrics.ass
# then burn onto the video with libass:
ffmpeg -i <video.mp4> -vf "ass=subtitles/lyrics.ass" -c:a copy out.mp4
```

`ass.py` matches timing words to the lyrics (no blind slicing), so it tolerates
the aligner tokenizing differently. Content lives in `lyrics.yaml`; all
placement/colour/motion lives in `effects.yaml` so it survives regeneration.

## Notes

There is of course no reason for anyone other than myself to want to run voxaline with these lyrics; these configs are stored
here for safe-keeping and for usa as reference for anyone who may want to run it. That said, if you are me or that anyone, note:  

- The `audio:`/`video:` paths inside `lyrics.yaml` point at voxaline's local
  test fixtures, not this repo's media — they matter only if you re-run
  `align.py`/`render.sh`. Point them at `../El_User.wav` / the video as needed.
- This repo's top-level `full_lyrics.md` has since been edited independently
  (section markers, a couple of word tweaks). `lyrics.yaml` + `timings.json`
  here are a matched set generated from the earlier lyric and are internally
  consistent — re-run interleave + align if you want them tracking the newer
  `full_lyrics.md`.

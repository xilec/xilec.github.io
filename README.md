# xilec.github.io

GitHub Pages for my projects, served at <https://xilec.github.io/>.

Currently hosts a single project page:

| Path | Project |
|------|---------|
| [`/RuVox/`](https://xilec.github.io/RuVox/) | [RuVox](https://github.com/xilec/RuVox) — local text-to-speech for Russian |

## Layout

```
/
├── index.html          # redirects to /RuVox/
└── RuVox/
    ├── index.html      # the landing page
    ├── style.css
    └── assets/
        ├── logo.svg
        ├── screenshot.png
        └── samples/*.wav
```

No build step: plain HTML and CSS, no dependencies. `.nojekyll` disables Jekyll
processing so assets are served verbatim.

## Theme

`data-theme` on `<html>` is one of `system`, `light`, `dark`. The switcher in the
header stores an explicit choice in `localStorage` under `ruvox-theme` (`system`
removes the key), and an inline script applies it before first paint to avoid a
flash. The dark palette is declared twice in `style.css` — once for the
`prefers-color-scheme` query and once for the explicit `dark` value — so the page
still follows the OS theme with JavaScript disabled.

The page-level `color-scheme` follows the theme, but the native `<audio>` widget
is drawn by the browser: on Linux with a dark GTK theme it can stay dark even on
the light page theme.

## Editing a project page

Each project lives in its own top-level directory and is deployed by pushing to
the default branch — GitHub Pages rebuilds automatically.

## Regenerating voice samples

Samples are recorded in the RuVox app (one and the same text per engine) and
committed as WAV, 48 kHz mono. To add or replace a sample:

1. Export the audio from RuVox.
2. Convert it to 48 kHz mono WAV and drop it into `RuVox/assets/samples/`,
   using a lowercase ASCII file name (`engine-voice.wav`).
3. Add a matching `<article class="sample">` block with an `<audio>` element in
   `RuVox/index.html`, or update the existing one.
4. If the spoken text changed, update the `.source-text` blockquote as well.

```bash
ffmpeg -i input.wav -ac 1 -ar 48000 -c:a pcm_s16le output.wav
```

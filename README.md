# Glass Icon Reveal (Nous fork)

A liquid-glass logo reveal, rebuilt as a single zero-dependency WebGL page.

This is a fork of [zanwei/glass-icon-reveal](https://github.com/zanwei/glass-icon-reveal) by Zanwei Guo. The glass engine is his: the page and its controls, the WebGL shader with its refraction, dispersion and corner focusing, and the spring timeline. This fork adds a wordmark reveal and a two-part logo sequence on top of it, made for Nous. See [What this fork adds](#what-this-fork-adds).

A water droplet condenses over a logo, spreads into a rounded glass tile, and lets go. The tile refracts the logo through its thick, rounded edge. Where the logo comes close to a corner, the edge draws it out into the rim with a warm line and a cyan color bleed. The flat face does not distort, so the logo itself stays straight.

**Original demo** (without this fork's additions): <https://zanwei.github.io/glass-icon-reveal/>

## Demo

The original tool:

https://github.com/user-attachments/assets/a77cc3b2-a01b-4dee-a560-cf5a6ed3e65d

## What this fork adds

- **Wordmark look.** A small round ball lens condenses in the middle of a word and bursts over it. The word's ends fan out in color, then break into fine vertical bands as the glass lets go.
- **Two-part sequence.** A look can play a second reveal once its own glass has let go. The first logo shrinks away into the middle, and the second reveal starts where it went, on the same backdrop.
- **Nous and Nous dark looks.** A mark condenses into a clean glass tile, lets go, and the Wordmark look then plays over a wordmark, in Nous's orange on warm white or on black. In this tile the corners bend light no harder than the sides, so the mark stays whole instead of being drawn out into the corners.
- **Placeholder marks.** The Nous marks are not part of this release. The Nous looks ship with a placeholder mark and a placeholder wordmark; replace them with your own.
- **Spectrum control.** Dispersion can keep a colored logo's own color: the light still bends and smears, without splitting into a rainbow.
- **Droplet rings.** Optional standing ripples inside the droplet.

## Usage

Open [`index.html`](./index.html) in a browser. There is no build step and nothing to install.

To serve it locally instead:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

- **Your own logo:** drop an SVG, PNG, JPG, or WebP onto the stage, paste an image, or choose **Logo → Upload…**. Choose **Text** to type a short word.
- **Scrub:** drag sideways on the stage. Double-click to replay.
- **Keyboard:** <kbd>Space</kbd> plays or pauses, <kbd>R</kbd> replays, <kbd>←</kbd> and <kbd>→</kbd> step one frame.
- **Export:** **Save PNG** saves the current frame. **Record Video** records one full reveal as MP4 or WebM at twice the tile size.

Your settings stay in the browser's local storage. **Reset** returns the current look to its defaults.

## Features

- Five looks: light glass on a light backdrop, dark glass with an icon plate, Wordmark, Nous, and Nous dark
- Droplet phase: a round ball lens that condenses, swells, and spreads into a squircle tile
- Thick-glass refraction at the rounded edge, from Snell's law on a quarter-round bevel; the flat face leaves the logo undistorted
- Corner focusing that draws nearby strokes into the corners, with 20-sample spectral dispersion for the warm rim line and the cyan bleed
- Diffuse rim light, an inner lip highlight, and a soft droplet shadow
- A spring-driven timeline (damping ratio and response) with continuous velocity and no stalls
- Logos become signed distance fields at load, so edges stay sharp at any magnification
- Support for Reduce Motion and Reduce Transparency

## Controls

| Group | Controls |
| --- | --- |
| Logo | Logo, original colors, tile size, logo scale, final logo scale, icon plate |
| Glass | Optical depth, dispersion, color bleed, double bevel, bevel width, corner smoothing, corner pull, corner width, corner focus, edge smear, stretch to corners, diagonal bias, droplet lens, rim light, outer glow, shadow, ripple |
| Motion | Bubble bounce, bubble response, bubble start size, start zoom, hold in glass, glass drift, focus pull, stroke swell, droplet rings, spectrum, loop pause |
| Colors | Background, logo, plate |

## How it works

`index.html` holds the whole tool: the page, the controls, and one WebGL 2 fragment shader.

1. **Logo field.** The logo is drawn into a 1024 × 1024 canvas and turned into a signed distance field with an exact Euclidean distance transform. A mipmapped color texture keeps multicolor logos.
2. **Timeline.** `frameAt(t)` turns time into shader uniforms. Each channel follows one spring or two overlapping springs, so speed never jumps.
3. **Refraction.** In the rounded edge, a straight-down sight line meets a surface tilted by θ, bends by θ − θr (Snell's law), and reaches the logo plane displaced by z · tan(θ − θr). The outer rim looks deepest, so nearby strokes are drawn out to the rim.
4. **Corners.** Around the corners on the rising diagonal, sight lines converge on one focal point, so a stroke near the corner spreads around it and down both edges.
5. **Color.** Twenty wavelength samples each refract with their own index. A spectrum curve weights them before they are summed.
6. **Light.** A diffuse rim glow, an edge hairline, and an inner lip highlight go on top, and the tile is mixed over the backdrop.

## Browser support

Current Chrome, Edge, Safari, and Firefox with WebGL 2. Video export uses `MediaRecorder` with `canvas.captureStream()`. It saves MP4 where the browser can encode it, and WebM otherwise.

## Repository layout

```text
index.html   The whole tool: page, controls, and shader
LICENSE      MIT License
```

See [`CONTRIBUTING.md`](./CONTRIBUTING.md) before opening a pull request.

## Disclaimer

This is an independent experiment inspired by Apple's Liquid Glass app icons. The Wordmark and Nous looks were also timed against the motion of Apple Music's new identity film. It is not affiliated with or endorsed by Apple. The sample logos are included only to show the effect; the marks belong to their owners. The Nous name and marks are not covered by the license below.

## License

[MIT](./LICENSE). Copyright (c) 2026 Zanwei Guo for the original tool, and (c) 2026 The Nous authors for this fork's changes.

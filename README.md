# Fillroke

Turns traced SVG icons whose lines are **filled shapes** into clean, editable **centerline strokes**.

Image tracing (Illustrator Image Trace, Potrace, Inkscape Trace Bitmap and so on) outlines every line as a filled shape. Fillroke finds the middle of each line and rebuilds it as a single stroked path, so you get proper strokes you can restyle instead of outlines you have to fix by hand.

**Use it online: https://notelex.github.io/Fillroke/**

## Run it

There is nothing to install or build. It is a single HTML file that runs entirely in your browser; your files never leave your computer, whether you use the online version or a local copy.

1. Open the link above, or open `index.html` in Chrome, Edge, Firefox or Safari (double-click it).
2. Drop one or more SVG files onto the page, paste SVG code, or click **Open SVG…**.
3. Adjust the settings if needed, then click **Download SVG** (or **Download all (.zip)** for a batch).

## What it does

- Detects the stroke width and colour from your file (both can be overridden).
- Keeps sharp corners sharp and curves smooth, with as few anchor points as possible.
- Continues lines straight through crossings and trims the small stubs tracing leaves behind.
- Works with dark, coloured and light-on-dark artwork, including Image Trace output with white background shapes.
- Keeps the original `viewBox`, `width` and `height`.

### Settings

| Setting | What it changes |
| --- | --- |
| Width / Color | Stroke width and colour. **Auto** / **Original** use what was detected. |
| Line ends / Joins | `stroke-linecap` and `stroke-linejoin`. |
| Segments | Curves, or straight lines only. |
| Smoothness | Higher gives fewer anchor points and calmer curves. |
| Sharp corner from | Bends sharper than this angle become hard corners. |
| Remove stubs shorter than | Trims little branches left at corners and blunt line ends. |
| Continue lines through crossings | Joins the two halves of a line that crosses another. |
| Combine everything into one path | One `<path>` instead of one per line. |
| Detail | Resolution the shapes are analysed at. Higher is more precise but slower. |
| Ink threshold | How different from the background a pixel must be to count as line. |

## How it works

1. Renders the SVG to pixels and separates the lines from the background.
2. Computes a Euclidean distance and feature transform, then peels the shapes from the outside in until only the centerline is left (distance-ordered homotopic thinning).
3. Turns that centerline into a graph, removes tracing noise, and decides which branches continue through each junction.
4. Finds corners, fits straight lines and cubic Béziers (Schneider's algorithm) within a tolerance, and places line ends where the original stroke ended.

## Limits

- One stroke width per file. If an icon mixes thick and thin lines, the most common width is used.
- Very sharp mitred points (under about 40°) can come out slightly rounded at the tip.
- Solid filled areas that are not lines produce odd strokes; it is meant for line art.
- Dots become tiny round-capped strokes, so they disappear with flat line ends.

## License

[MIT](LICENSE)

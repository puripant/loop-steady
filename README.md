# Loop Steady

![Loop Steady](assets/og-image.png)

A single-page web app that steadies a handheld video so it loops without a visible jump. It runs entirely in the browser, so the video is never uploaded.

**[Open the app](https://puripant.github.io/loop-steady/)** (after GitHub Pages is turned on)

## How it works

1. **Read frames:** the clip plays through once. Each frame is stored as a small grayscale image, plus a colour thumbnail.
2. **Mark what should stay still:** you drag a box on the first frame. Up to 64 detailed spots inside it are matched against the first frame in every later frame. The matching looks for the best 15×15 patch fit, is robust to brightness changes, and works to sub-pixel accuracy.
3. **Fit the camera motion:** a shift, tilt and zoom is fitted to the matched spots. Spots that disagree, like something moving through the box, are rejected. Because every frame is compared with the first one, errors don't add up over the clip.
4. **Plan the steadied camera:** *Lock* holds the camera completely still. *Smooth* uses a looping Gaussian blur of the motion, so the end joins the start. *Ends only* removes the gap between the last and first frames.
5. **Crop:** finds the largest rectangle that stays inside every corrected frame, in either the original shape or the largest area.
6. **Export:** draws each corrected frame to a canvas and records it with `MediaRecorder` at the chosen speed (0.25×–4×), with optional sound.

It also includes a cut-parts overlay and a space-time cube that shows the raw camera path next to the straight crop tube.

## Run locally

It's one static file with no build step. Serve the folder and open `index.html`:

```sh
python3 -m http.server 8000
```

For best results use Safari on iOS 15+ or a recent Chrome. Short clips of 2–20 seconds work best.

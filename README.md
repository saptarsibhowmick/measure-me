# MeasureMe — Bespoke Trouser Measurement Assistant

A browser-based body measurement tool built for bespoke tailoring. Point a camera at yourself, strike two poses, and get all 14 measurements your tailor needs — entirely on your device, with no data ever leaving it.

**Live app → [saptarsibhowmick.github.io/measure-me](https://saptarsibhowmick.github.io/measure-me/)**

---

## What it does

MeasureMe uses Google's **MediaPipe BlazePose** to detect 33 body landmarks in real time from your device camera. From two poses — front and right side — it derives the complete set of measurements needed for a classic full-cut bespoke trouser, including:

- Natural waist, seat, thigh, knee, and ankle circumferences
- Outseam, inseam, full rise (front and back), knee height, finished trouser length
- Crotch curve, waistband width, and cuff depth

Every measurement is labelled by confidence: **Measured** (directly from landmarks, ±1–2 cm), **Estimated** (derived from body proportions, ±2–4 cm), or **Fixed** (per the tailor brief specification).

Results can be printed or saved as a PDF to hand directly to your tailor.

---

## How to use

**Prepare**
- Wear only briefs, socks, and shoes — clothing adds bulk and skews results
- Prop your device 2–3 metres away at hip height (a ledge, tripod, or stack of books)
- Stand in front of a plain, evenly lit wall — avoid backlighting
- Keep arms slightly away from your sides

**Capture**
1. Enter your height in centimetres — this is the scale calibration reference
2. Press *Begin session*
3. **Front view** — face the camera; wait for the gold skeleton overlay and the *Ready to capture* status badge, then press the shutter and hold still for the 3-second countdown
4. **Side view** — turn so your right side faces the camera; repeat the same process
5. Your 14 measurements appear immediately

---

## Technology

| Component | Detail |
|---|---|
| **Pose detection** | [MediaPipe BlazePose](https://google.github.io/mediapipe/solutions/pose) (model complexity 0) |
| **Licence** | Apache 2.0 — free for personal and commercial use |
| **Processing** | Entirely on-device; no images, landmarks, or measurements are transmitted |
| **Dependencies** | Zero npm packages; one CDN script tag |
| **Hosting** | Single static HTML file — no server, no build step |
| **Browser support** | Any modern browser over HTTPS (Safari iOS 14.5+, Chrome 90+, Firefox 90+) |

### Measurement methodology

**Lengths** are computed directly from pixel distances between BlazePose landmarks, scaled by a pixel-per-centimetre ratio derived from the user's entered height. These are the most reliable outputs (±1–2 cm).

**Circumferences** combine the front-view width of each body segment with the side-view depth, then apply the Ramanujan ellipse approximation to estimate the full circumference. These carry a larger margin (±2–4 cm) and are intended as a tailor's starting draft rather than a final measurement.

---

## Accuracy & limitations

- The camera is 2D. Circumferences are **estimated**, not wrapped. A tape measure at the fitting will always be more accurate.
- Accuracy depends on good lighting, a plain background, and a device held at hip height 2–3 m away.
- The app requires the user to enter their height — without a real-world reference, pixel distances cannot be converted to centimetres.
- A toile (muslin mock-up) fitting is still strongly recommended before cutting final fabric, especially for high-rise full-cut trousers where the rise and crotch curve are critical.

---

## Tailor brief

This tool was built as part of a full bespoke tailoring project. The measurements it produces correspond directly to the construction brief for a **classic full-cut formal trouser** in the mid-century British tailoring tradition, specifying:

- High-rise waistband at the belly button
- Functional double forward pleats
- Deep on-seam side pockets
- Side adjusters, no belt loops
- Cuffed hem (turn-ups), single break, covering two-thirds of the shoe
- One welt pocket (right side only), single button fastening
- Half-lined back panels in Bemberg (cupro)
- Hook-and-bar closure

---

## Running locally

No build step or package manager required. Clone the repository and open `index.html` directly — or serve it over a local server for camera access:

```bash
# Python 3
python -m http.server 8080
# then open http://localhost:8080 in your browser
```

Camera access requires either `localhost` or a proper `https://` origin. Opening the file via `file://` will cause camera permission errors in most browsers.

---

## Repository structure

```
measure-me/
└── index.html    # complete self-contained app — all HTML, CSS, and JS in one file
└── README.md
```

---

## Privacy

No data leaves your device. The app:
- Does not transmit camera frames
- Does not store landmarks or measurements
- Does not use cookies or local storage
- Does not load any analytics or tracking scripts

The only external requests are the MediaPipe BlazePose model files, fetched once from `cdn.jsdelivr.net` on first run (~5 MB) and cached by the browser thereafter.

---

## Licence

MIT — do whatever you like with it. Attribution appreciated but not required.

# Sugar Counter

Date: 2026-10-07
Status: waiting for review
Game folder: `sugar-counter/index.html`
Audience: Falcon, ages 2 and up, the same range as Pop Pals

## Purpose

Sugar Counter is a one-page bakery counter where a child decorates a donut. The motor lesson is the difference between a tap and a drag. A tap leaves one mark. Holding the press and moving draws a line. The Tobii Eye Tracker 5 can do both acts too: a steady look leaves one mark, and moving the look after it latches draws the line.

Success for the first version: with a mouse or a finger, he can drop a glob and draw a stripe; with the tracker connected, a steady look drops a glob and a moving look draws a stripe; a picture order can be filled by those marks and is never cleared because the marks were wrong.

Pop Pals stays as it is. This game does not share code with it.

## What this version is

One donut station. The donut sits in the middle and does not move. A tray holds four icing colors (pink, chocolate, white, blue), a sprinkle jar, and a cherry. A Done control serves the treat. Two ways to play, chosen on the start screen:

- **Decorate** is free play. Done celebrates and brings a blank donut.
- **Orders** shows a picture card. Done serves only when every picture on the card is filled. An early press keeps the donut and the card.

The page is one HTML file with a canvas and a few real buttons. It opens from `file://` or `http://localhost`. No build, no image files, no score, no account, no timer.

## What this version is not

Soft-serve, scoops, cakes, cupcakes, a speed round, saving a picture, blink or wink gestures, and a leaderboard wait for later specs. Gaze cannot be asked to carry a scoop onto a cone. That station, when it exists, drops a scoop by looking at a flavor and then at the cone. The mouse can drag. This spec does not build that station.

## Inputs

The donut understands four actions: pick a tool, drop one mark, draw a stroke, let go. Mouse, touch, and gaze all produce those actions. Marks already on the donut stay there.

Pink icing is selected when a round starts.

### Mouse and touch

Pointer Events on the canvas and the buttons. `touch-action: none` on the play surface so the browser does not scroll. One pointer at a time. A second contact is ignored until the first one ends. The pointer is captured on press.

- Pressing a tray button selects that tool and does not mark the donut.
- Pressing the donut with icing or sprinkles begins a mark at the press point.
- Releasing without moving 18px from the press point keeps a single mark (a glob, or one sprinkle cluster).
- Moving 18px or more draws a line. Points are kept when the pointer has traveled 0.45 blob diameters from the last kept point. The line still scores as one glob or one cluster until its path reaches two blob diameters. At that length it scores as one stripe and the glob or cluster count for that gesture goes away.
- Releasing ends the gesture. A finished line shorter than two blob diameters scores as one glob or one cluster, not as a stripe.
- A press outside the donut, and outside the buttons, makes no mark.
- A cherry press places one cherry and moves that same cherry until release. Released inside the donut, it counts. Released outside, it is removed and does not count. A second cherry needs a new press.
- Tray presses during a capture are ignored until release.

The icing nozzle is drawn a blob-radius above a touch point so the finger does not cover the line. A mouse uses the pointer point itself.

### Gaze

The page uses the same daemon contract as Pop Pals.

- Socket: `ws://127.0.0.1:7081`. An `https` page will not open it.
- On open, send the Pop Pals subscribe frame: bytes `1, 4, 0, 0, 0, 0, 5, 0, 0`.
- Framing is `[u8 type][u32 little-endian length][payload]`. Type `1` with a payload of at least 56 bytes is a gaze sample. Type `4` is the 3×3 screen homography (nine little-endian f64 values, 72 bytes). An empty type `4` clears the homography. Anything else is ignored.
- A sample is used when the validity mask has bit 64 set, at least one eye is seen (validity 0), and both x and y are finite and ≥ 0. The homography is applied before the point is scaled by `window.innerWidth` and `window.innerHeight`. A non-fullscreen window still maps the whole calibrated panel onto the window. Play fullscreen on the panel that was calibrated.
- Reconnect 2 seconds after a close or a failed constructor. A bad length above 1 MiB resets the buffer.
- The Eyes chip is shown when the socket is open or a test look is held. The ring is shown only while a sample is fresher than 500 ms.
- There is no blink or wink event. A gap in samples is only a gap in samples.

Dwell times match Pop Pals: 650 ms on the donut, 1300 ms on tray buttons, Done, and start-screen buttons. The ring fills during a dwell (`--p` from 0% to 100%). Changing target resets the dwell. The ring sits above every other layer (`z-index: 60`) and does not receive pointer events.

Gaze squeeze is a latch:

1. Looking at a tray tool for 1300 ms selects it. A shorter look does not.
2. Looking at the donut for 650 ms with icing or sprinkles latches the squeeze. A blob or a sprinkle cluster appears at once and the count includes it. This is the click.
3. If the smoothed look then travels past the gaze dead zone, a line follows the look. This is the drag. The gesture still scores as one glob or one cluster until the path reaches two blob diameters. At that length it scores as one stripe and that gesture's glob or cluster is removed from the count.
4. A cherry dwell places one cherry on the donut and does not latch.
5. The latch releases when any of these happen: the smoothed look stays outside the donut for 280 ms, the tracker has no fresh sample for 500 ms, a pointer goes down, or Done is activated. A gesture that never reached stripe length keeps its single glob or cluster. Release does not add a second one.
6. A gap shorter than 500 ms pauses new points and keeps the latch. The line is not bridged across the gap.
7. Looking at the tray, Done, Mute, or Fullscreen is outside the donut, so the 280 ms leave timer starts. Those dwells are 1300 ms, so the latch releases before the button fires.

The smoothed look is what the ring, the hit test, and the icing all use. The first sample snaps to the raw point. Each later sample moves the smoothed point 45% of the way toward the raw point. The gaze dead zone is the larger of 28 px and half a blob diameter, measured from the point where the latch fired. Motion inside that zone moves the current mark and still scores as one mark.

While any pointer is down, gaze dwell progress stays at zero and gaze does not add points. A pointer press releases a latch first, then starts its own mark. One press adds one pointer mark.

## The counter

The canvas covers the window and is resized to the live window size, with a backing-store ratio capped at 2. Each mark is stored as an offset from the donut center, in fractions of the donut's horizontal and vertical radii, so a resize keeps it on the same spot of the donut.

Regions:

- The tray band is the bottom strip, height `clamp(120px, 18% of the canvas height, 180px)`.
- When the canvas is at least 800 px wide, the order card is a 220 px column on the left. In Decorate mode that column is absent.
- When the canvas is narrower, the order card is an 88 px strip at the top of the canvas. In Decorate mode that strip is absent.
- The play rect is the canvas minus the tray and minus the card. The donut is centered in the play rect. Its horizontal radius is 42% of the play rect's short side, and its vertical radius is 36%, reduced so the ellipse stays 12 px inside the play rect.
- The hole is drawn at 38% of those radii. It is paint only. Looks and presses in the hole still hit the donut.
- The donut hit target is the full ellipse.
- Tray cells are circles in one row, six of them: pink `#e86a9a`, chocolate `#6b3a2a`, white `#fff6ea`, blue `#6eb5e8`, sprinkles, cherry. The diameter is whatever fits those six in the tray width with 8 px gaps, and it is capped at 112 px. They are real `<button>` elements with names and `aria-pressed` on the selected tool.
- Done is a real button at the top right, clear of the donut. Its shorter side is 96 px when the canvas short side is at least 700 px, and 64 px otherwise.
- Mute and Fullscreen are real buttons at the top left during play and on the start screen. Each shorter side is at least 72 px. Both are 1300 ms gaze targets. Fullscreen toggles the document element. If the browser refuses, the button does nothing else.

A blob's radius is the larger of 18 px and 4.5% of the canvas short side. Sprinkles are a cluster of 7 short marks. A cluster is repeated along a stroke every 0.7 blob diameters. Icing points along a stroke are spaced at 0.45 blob diameters.

A line scores as a stripe only when its path is at least two blob diameters long. Below that length the gesture scores as a single glob or a single cluster.

The donut stores at most 400 icing blobs, 80 sprinkle clusters, and 24 cherries. Past a cap, new marks of that kind are not added. Sounds still play. A stroke that already passed the stripe length still counts.

Drawing order is donut, then icing and sprinkles in the order they were made, then cherries on top. `prefers-reduced-motion: reduce` draws a single flash when a treat is served and skips particle motion. Marks themselves still appear.

Colors: paper `#fff3e4`, ink `#3a2a22`, counter `#f3d2a8`. Shapes are canvas drawings. No external art.

## Orders

Orders cycle in this order, then repeat from the first. There is no random draw.

| # | Card |
| --- | --- |
| 1 | 3 pink globs |
| 2 | 1 chocolate stripe, 1 cherry |
| 3 | 2 sprinkle clusters, 1 white glob |
| 4 | 2 blue globs, 1 cherry |
| 5 | 1 pink stripe, 1 sprinkle cluster |
| 6 | 1 white stripe, 2 cherries |
| 7 | 3 sprinkle clusters |
| 8 | 1 chocolate glob, 1 blue glob, 1 cherry |

A glob fills a glob pip of that color only. A stripe fills a stripe pip of that color only. A sprinkle cluster fills a sprinkle pip. A cherry fills a cherry pip. Extra marks fill nothing further and remove nothing. A stripe never fills a glob pip. A glob never fills a stripe pip.

The card shows the pictures and one pip per required mark. Filled pips stay filled. A simple face smiles when every pip is filled. The face is not a target.

Done before the card is full plays a short waiting sound, wiggles the card, and leaves the donut. Done when the card is full plays the serve sound, clears the donut, and advances the card. In Decorate mode, Done always clears the donut after the serve sound.

There is no separate undo. A mess is finished by adding the marks the card still wants.

## Sound

WebAudio oscillators only, generated in the page, started after the first successful `AudioContext` resume. Resume is attempted on the first pointer press and on the first gaze action. If the browser blocks the gaze attempt, audio begins on the first tap after that. Mute toggles sound.

The start screen says "Press and move on the donut to ice it." While the Eyes chip is showing, that line says "Look at the donut until the ring fills, then move to draw."

Separate sounds: tool change, glob, stroke tick (at most one every 80 ms), sprinkles, cherry, latch release, serve, and the waiting sound.

## Test hook

`window.sugarCounter` is available as soon as the script runs.

- `start("free" | "orders")` begins that mode. Orders begin at card 1. After card 8 is served, the next card is 1 again.
- `look(nx, ny)` holds a gaze point in window fractions, the same idea as Pop Pals `look`. `releaseLook()` clears it.
- `dwell()` returns the ring fill from 0 to 1.
- `eyes()` returns `{ on, live }`.
- `tap(nx, ny)` is a press and release at window fractions.
- `drag([{x, y}, ...])` presses at the first point, moves through the rest, and releases at the last. Fractions are of the window.
- `centers()` returns the donut ellipse and each tray and Done target in window pixels and in window fractions.
- `state()` returns the mode, the selected tool, whether the latch is on, mark counts by kind and color, the order index, and whether every pip is filled.
- `order()` returns the current card's requirements and filled counts.
- `step(now)` advances gaze and animation to a timestamp in milliseconds. Tests pass time in. They do not wait on the real clock.
- `selfCheck()` runs the cases below and returns `{ ok, failures }`. Each failure is one sentence naming the case. For the whole run, socket samples are ignored. When the run finishes, the start screen is showing, pink is selected, the donut is empty, and live samples apply again.

`selfCheck` covers:

1. A donut tap inside 18 px counts as one pink glob and no stripe.
2. A drag of at least two blob diameters counts as one pink stripe and no glob.
3. A 650 ms look that stays inside the gaze dead zone counts as one pink glob and turns the latch on.
4. Moving that latched look by at least two blob diameters counts as one pink stripe and no glob, and leaves the latch on.
5. Staying outside the donut for 280 ms turns the latch off and keeps a single provisional glob.
6. A 200 ms sample gap keeps the latch and adds no points during the gap.
7. 500 ms without a sample turns the latch off and keeps the single provisional glob.
8. A pointer press during a latch turns the latch off, then that press can add its own glob.
9. A 1300 ms look at a tray tool selects it. A 600 ms look does not.
10. Order 1 fills from three pink globs and does not fill from a pink stripe.
11. Order 2 fills from one chocolate stripe and one cherry.
12. Done on an unfinished order does not advance the card or clear the marks.
13. Done on a finished order advances the card and clears the marks.
14. One sprinkle tap counts as one cluster. A cherry drag that ends on the donut counts as one cherry. A cherry released outside the donut does not count.
15. With the socket closed and no held look, the ring is hidden and a tap still adds a glob.

## Files

| Path | Change |
| --- | --- |
| `sugar-counter/index.html` | The game |
| `README.md` | One section beside Pop Pals: how to open the file, the ages, the socket, fullscreen on the calibrated panel, and the `https` limit |

The gaze client is copied into the new file as a small set of functions. It is not extracted into a shared module.

## Later, not in this spec

A soft-serve station can reuse the latch: looking at the machine fills the cone while the latch is on. A scoop station places a scoop when the look dwells on a flavor and then on the cone, while the mouse drags the scoop. A timer mode, if it is ever added, starts only after Orders exists and uses a generous clock that does not clear the donut on expiry. Each of those is its own spec.

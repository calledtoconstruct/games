# Math Practice

**Date.** 2026-10-18.

**Status.** Build spec. One implementer, one file, no leftover choices.

**File.** `/home/joseph/code/games/math-practice/index.html` only. Inline style and one inline script. No second script, no module, no asset folder.

**Window.** `window.mathPractice`, assigned as soon as the script runs, before the query string is handled.

**Title.** Math Practice.

**License.** The repo MIT license. Copyright 2026 Joseph Woolley. This file adds no second license and no different copyright line.

The child fills one missing value from large cards. Mouse, touch, and gaze do that same act. A wrong card leaves the problem up. There is no score and no clock on screen. This game never speaks.

## Player loop

The page opens on the menu. The highlighted band is Tiny Tots. Let's go, or `start(band)`, deals one sitting and opens the ask screen.

A sitting is a sequence of problems. Each problem shows one stem and two, three, or four cards. Exactly one card completes the stem. The child dwells a card, or clicks or taps it. There is no second step, no drag, and no keypad.

- A right card writes that value into the blank. The finished stem stays up for 700 ms. Then the next problem appears, or the rest card appears when the band cap is reached.
- A wrong card does not write, does not reshuffle, does not advance, and does not scold. That card ignores a held look until the look leaves it. The other cards stay live.
- Done, on the ask screen, ends the sitting at once and opens rest. The cap is a ceiling, not a quota. Done on the first problem is allowed.
- Rest shows one calm sentence and Another. Another starts the same band over, with a new seed and an empty history. Back to Menu returns to the menu and drops the sitting. Nothing is stored.

The ring is the only progress paint. It fills across the whole dwell: 0 at entry, 1 at 650 ms on a card and at 1300 ms on a menu button. There is no onset timer. A look of 640 ms does not commit. A look of 660 ms does. A click or a tap commits with a wait of 0 ms and runs the same writer as a finished dwell.

## Bands

Band ids, matching the menu buttons: `tiny`, `little`, `shop`, `rush`. Do not rename them. Do not drop Rush. Do not add a fifth band.

| Band | Ages on the button | Cap | Cards | What is on the stem | Operations |
| --- | --- | --- | --- | --- | --- |
| Tiny Tots | 2 to 3 | 4 | 2 | Apple counts. No digit and no operator glyph | None. `op` is null |
| Little Pals | 4 to 5 | 6 | 3 | Digits, blue dots, `+` or `−`, `=` | `+` and `−` only, totals inside 0–5, layout always right |
| Shopkeeper | 6 to 8 | 8 | 4 | Digits and `+` or `−`. No dots | `+` and `−` only, one index schedule, never `×` or `÷` |
| Rush | 9 and up | 10 | 4 | Digits and one of `+`, `−`, `×`, `÷` | All four, one index schedule, still three terms and one blank |

Caps and card counts are `mathCap` and `mathCardCount`. They do not change because the window is short. Do not drop a card. Do not shrink a card to pretend a window under 320 px wide fits.

Every band has one blank. The child fills it from the cards. Compare problems, two-step problems, parentheses, and remainders are not generated.

### Pictures and type

Every picture is drawn on a canvas in this file. Flat fills, high contrast, no letters, no logo, no face, no downloaded file.

**Apple**, used for Tiny Tots only. A red circle `#d6453d`, a short brown stem `#6b3a22` centered on top, and one green leaf `#2f8a3e` at the stem. Every apple is the same size and the same drawing. An empty place is the same circle as a stroke only, `#2c2416`, cream inside, no stem and no leaf.

**Set** (`blank` `"set"`). Two trays of equal size. The first holds `a` apples. The second is empty. The empty tray is the blank and is not a button. The right card is `a` apples, the same count as the filled tray.

**One more** (`blank` `"more"`). One row: `a` apples, then one empty outline. The outline is the blank and is not a button. The right card is `a + 1` apples.

Counts are only 1, 2, and 3. Cards draw that many apples and nothing else. Button text is empty. The accessible name is "One apple", "Two apples", or "Three apples". No digit character is painted or placed in the ask screen's text.

**Little Pals type.** Large black digits `#2c2416`. To the right of each digit, that many blue dots `#3d8fd4`. A 0 has an empty dot row with the same height, not a missing row. Glyphs are `+`, `−` (U+2212), and `=`. The blank is an empty rounded square, cream fill, ink stroke, about the digit size. It is not a button. Each card is the digit plus its dots. No apples.

**Shopkeeper type.** Large digits and `+` or `−` only. No dots. No `×`. A two-digit answer is one card. The repeated line is three digits joined by `+`, then `=`, then the blank: `4 + 4 + 4 = □` when `a` is 4. It is not a times glyph.

**Rush type.** Large digits and one operation glyph. `×` is U+00D7. `÷` is U+00F7. A two-digit or three-digit value, including 100, is one card. No dots.

**Right layout** prints `a op b = c`, with the one null drawn as the blank square. **Left layout** prints `c = a op b` in that order. Little Pals never use left layout. Shopkeeper and Rush use left layout only where the schedule says `left`.

On a correct show, the blank becomes the numeral, or, for Tiny Tots, the empty tray fills with `a` apples, or the outline becomes an apple so the row shows `a + 1` apples. Snap into place. Do not animate when `prefers-reduced-motion: reduce` is set. The 700 ms wait still happens.

### Menu blurbs

The menu card shows the title, the line "Look at the card that fills the hole.", the line "Ages 2 and up", then the four band buttons and a sun-gold Let's go button.

| Button | id | Small line |
| --- | --- | --- |
| Tiny Tots | `tiny` | Ages 2 to 3. Apples, one or two or three. No clock. |
| Little Pals | `little` | Ages 4 to 5. Adding and taking away, up to 5. No clock. |
| Shopkeeper | `shop` | Ages 6 to 8. Plus and minus, up to 20. |
| Rush | `rush` | Ages 9 and up. Times and divide, still one hole. |

Those lines do not contain the characters `+`, `−`, `-`, `=`, `×`, or `÷`. The highlighted button has `aria-pressed="true"`. The others have `false`. Tiny Tots starts pressed.

## Screens and API

### Document

The head holds charset utf-8, a viewport meta, `<meta name="theme-color" content="#f4f0d8">`, and `<title>Math Practice</title>`. Nothing else is loaded. No link, no `@import`, no image URL, no font URL, no script URL.

Font stack: Arial Rounded MT Bold, then Trebuchet MS, then Segoe UI, then sans-serif. Cream page `#f4f0d8`, card `#fffdf6`, ink `#2c2416`, sun-gold `#ffc857`, sun press `#e2a62e`.

The body, in this order:

1. `<canvas id="view">`, full page, `pointer-events: none`. It draws stems and apples. It is never the gaze hit.
2. Chrome, always in the tree: `#mute`, `#full`, `#back`, `#eyes`, `#gaze`, `#banner`.
3. `#screen-menu`.
4. `#play`, which holds the stem, the card buttons, and `#done`.
5. `#screen-rest`, which holds the calm sentence and `#another`.

`#selfcheck-result` is not in the markup. The loader creates that `pre` only when `selfcheck` is present.

`go` shows one of `menu`, `ask`, and `rest`, and hides the other two. Chrome stays. `#back` is hidden on the menu and shown on ask and rest. `#done` is shown only on ask. `#another` is shown only on rest. The stem and the cards are shown only on ask.

### Chrome

`#mute` is 72 by 72 px, at left 8 px and top 8 px. Visible text is empty. It contains two inline SVGs, one hidden at a time. The on glyph is a speaker with one sound arc. The off glyph is the same speaker with a cross. `aria-pressed` is `false` when sound is on. `aria-label` is "Mute sound" when sound is on, and "Turn sound on" when muted. The words Mute and Sound are not visible text.

`#full` is 72 by 72 px, at left 88 px and top 8 px. Visible text is "Full". `aria-label` is "Fullscreen". It toggles `document.documentElement.requestFullscreen`. If the browser refuses, the button does nothing else.

`#back` says "Back to Menu". Height 72 px, short side at least 72 px, at top 8 px and right 8 px. It calls `go("menu")`.

`#eyes` is a div, not a button, and not a target. Its only text is "Eyes". It is shown when the socket is open or a test look is held, and hidden otherwise.

`#gaze` is the 54 px ring. It is not a button.

`#banner` is one short sentence, `pointer-events: none`, not a score, a timer, or a rank. Set it with `textContent`.

| Screen | Banner |
| --- | --- |
| menu | Look at the card that fills the hole. |
| ask, quantity | How many apples? |
| ask, equation | Fill the hole. |
| show | There it is. |
| rest | That is enough for now. |

The rest card repeats "That is enough for now." It does not show a grade, a count of misses, a time, a star, or the index.

No screen shows the index, the cap, a score, a streak, a mistake total, or a clock.

### Hook

`window.mathPractice` has these functions and no second parent mode.

- `start(band)` calls the shared band writer. If `band` is not one of the four ids, it does nothing and writes nothing into the page. Otherwise it sets the band, resets the sitting, deals problem 0, and calls `go("ask")`. It does not open the socket.
- `go(screen)` is the only writer of the screen id. Unknown ids do nothing. Helpers may call `go`. They may not assign the screen themselves.
- `look(nx, ny)` places a test gaze at window fractions. It does not commit.
- `releaseLook()` drops the test gaze and clears a cooled card.
- `hold(nx, ny)` is a pointer down. A second hold is ignored until `release`.
- `release()` is the matching pointer up. It does not fire a button.
- `tap(nx, ny)` runs the writer for the button at that point, with no dwell, and does not leave the pointer down. If a hold is already active, `tap` does nothing.
- `step(now)` is the only clock.
- `dwell()` returns the ring fill from 0 to 1.
- `eyes()` returns `{ on, live }` and nothing else. No coordinates.
- `centers()` returns window fractions and pixel boxes for every visible button.
- `targetsAt(w, h)` returns those boxes for a pretend viewport.
- `state()` returns plain data.
- `seed(n)` pins the next sitting only. It does not change a sitting that has already started. It is not read from the query. The stored pin is `n >>> 0`.
- `selfCheck()` returns `{ ok, failures }`. Each failure is one sentence. The name of the case is the first word, then a colon.
- `answer(value)` is the only writer of the blank.
- `next()` leaves show for the next problem, or calls `go("rest")` at the cap.

`render` runs before any of these return, including while the check flag is on and the frame loop is off.

`start`, a band button, and Let's go share one band writer. A band button only highlights. Let's go calls `start` with the highlighted id. A card button, `tap`, and a committed look share `answer`. Done calls `go("rest")`. Another calls `start` with the current band.

### state()

`state()` can be turned into JSON. It always includes `level`, `screen`, `banner`, and `muted`. It never includes a DOM node, a gaze coordinate, the socket URL, the query string, a seed, a history array, or the answer.

| Key | Values |
| --- | --- |
| `level` | `"tiny"`, `"little"`, `"shop"`, or `"rush"`. On the menu this is the highlighted band, starting as `"tiny"`. |
| `screen` | `"menu"`, `"ask"`, or `"rest"` |
| `banner` | The sentence in the table above |
| `muted` | boolean, default false |
| `phase` | `null` on menu and rest, otherwise `"ask"` or `"show"` |
| `kind` | `null`, `"quantity"`, or `"equation"` |
| `op` | `null`, `"+"`, `"-"`, `"×"`, or `"÷"`. The state string for minus is the ASCII hyphen-minus. The painted glyph is still U+2212. |
| `a`, `b`, `c` | The terms. A null is the blank, except a quantity, which stores the shown count in `a` and leaves `b` and `c` null. |
| `blank` | `null`, `"set"`, `"more"`, `"result"`, `"part"`, or `"start"` |
| `cards` | The integers on the cards, in painted order, or `[]` when no problem is up |
| `filled` | `null` while the blank is empty, otherwise the integer written there |
| `index` | 0-based index of the problem on screen. 0 on the menu. On rest after a finished cap, the cap. |
| `cap` | `mathCap(level)` once a sitting exists, otherwise `null` |
| `layout` | `null`, `"pictures"`, `"right"`, or `"left"` |
| `repeats` | `null` when no problem is up, otherwise `1`, or `3` for the repeated addend |
| `range` | `null`, or `[lo, hi]` inclusive |

Forbidden keys, anywhere in the object: `wrongs`, `score`, `lives`, `streak`, `clock`, `msLeft`, `said`, `speech`, `answer`, `history`, `seed`.

### answer, next, and the show beat

`answer(value)` does nothing unless `phase` is `"ask"`, `value` is a number, and `value ===` one of the current cards. A string does nothing and is not written into the page. A value that is not on the cards does nothing.

The writer also drops the call when that card is cooled and this visit has not left it. Cooling is a flag on the writer, not a second timer in the gaze client.

- Right value: `mathAccepts` is true. Set `filled` to that value, set `phase` to `"show"`, and append `{ blank }` to the sitting history. Record `showUntil` as the `step` time of the commit plus 700. Do not increment `index` yet. Play the correct tone.
- Wrong value: leave `a`, `b`, `c`, `blank`, `cards`, and `filled` as they were. `filled` stays null. `phase` stays `"ask"`. Cool that card. Do not append history. Do not call `mathMake`. The banner stays the ask sentence. It does not contain "fail", "wrong", "miss", or "no".

`answer` during `"show"`, on the menu, or on rest does nothing.

`next()` does nothing unless `phase` is `"show"`. If `index + 1 >= cap`, call `go("rest")`. Otherwise increment `index`, call `mathMake` with the history already appended, and show that problem in `"ask"` with `filled` null. If `mathMake` returns null, call `go("rest")`.

`step` calls `next` when `phase` is `"show"` and `now >= showUntil`. The comparison uses the absolute `step` timestamp. It is not an accumulated sitting clock, and the 120 second gap clamp does not block it. Reduced motion does not shorten the 700 ms.

`SHOW_MS` is 700.

### Seed

One sitting has one uint32 seed, chosen inside `start`.

- If `seed(n)` was called since the previous sitting began, use that pin and clear it.
- If the self-check is running and nothing is pinned, use `playSeed(10000, counter)` so the check does not read the wall clock.
- Otherwise use `playSeed(Date.now(), counter)`.

`counter` starts at 1 and increases by 1 for each sitting seed taken. `mathMake` for every index in that sitting uses the same sitting seed. `seed(n)` after `start` only pins the next `start`.

### Sound

An oscillator may start after the first pointer press or the first committed gaze action. During `selfCheck`, do not construct an `AudioContext`.

Master gain is 0.5 when unmuted and 0 when muted. Mute does not remove buttons. Only a correct `answer` plays a tone: sine, 523 Hz, 120 ms. A wrong card, a menu commit, and rest play nothing. There is no buzzer and no recorded file.

### Query

Flags are presence checks. The value is ignored. The raw search string is never written into text or an attribute.

Handled only when `selfcheck` is absent:

- `play` calls `start("tiny")`.
- `rush` calls `start("rush")`.

Any other flag does nothing. `selfcheck` does not apply `play` or `rush`.

### Loader

If the query contains `selfcheck`, set `checking` and `suppressReconnect`, do not call `requestAnimationFrame`, and do not construct a `WebSocket`. If a socket already exists, close it and keep reconnect suppressed. Call `selfCheck()`, create `<pre id="selfcheck-result">`, set its `textContent` to `JSON.stringify` of the result, set `document.title` to `SELFCHECK_OK` or `SELFCHECK_FAIL`, and stop.

Otherwise apply the two deep links, then start the frame loop and `connectGaze`.

The frame loop calls `step(performance.now())` only when not checking. `performance.now()` does not decide a dwell during the check. The check drives time only through `step`, from 10000.

### Storage and strings

Do not call `localStorage.setItem` or `localStorage.removeItem`. Do not read storage except the storage case below. Do not set a cookie, send a beacon, or `fetch`. No `eval`, no `new Function`, and no timer that takes a string. No `innerHTML`, `insertAdjacentHTML`, or `document.write`. Labels, banners, and card numerals are authored or generator integers assigned with `textContent`. The source of this file does not contain the identifier `speechSynthesis`.

## Gaze targets

Copy the gaze client shape into this file. Do not add a second socket, a second dwell, or a shared script.

### Constants

Spell these names:

| Name | Value |
| --- | --- |
| `GAZE_URL` | `ws://127.0.0.1:7081` |
| `PLAY_DWELL_MS` | 650 |
| `MENU_DWELL_MS` | 1300 |
| `GAZE_FRESH_MS` | 500 |
| `LEAVE_MS` | 280 |

`LEAVE_MS` is declared and is 280 so the constant block matches the other games. Math has no held stroke and no boost latch. Do not add 280 to 650 or to 1300. Do not delay a cancel by 280 ms. Do not use it as an onset. When `step` sees a different button, or no button, the in-progress visit is discarded on that step and the fill returns to 0.

### What is a target

Every selectable thing is a real `button`. Hit testing is `elementFromPoint`, then the nearest `button`. A hidden button or a button with no box is not live.

| Control | `data-play` | Dwell |
| --- | --- | --- |
| Card buttons `#card-0` … | `"true"` | 650 ms |
| `#lets-go`, `#tiny`, `#little`, `#shop`, `#rush` | absent | 1300 ms |
| `#done`, `#another`, `#back`, `#mute`, `#full` | absent | 1300 ms |

The stem, the trays, the apples, the digits in the equation, the blank square, the banner, the canvas, and `#eyes` are not targets. Looking at the problem selects nothing.

There is no ten-key, no `input`, and no `textarea`.

### Clock

One pointer. It is the mouse, the touch, or the smoothed gaze. The first sample snaps to the raw point. Each later sample moves 0.45 of the way toward the new point. A test `look` snaps when no smoothed point is stored. Later `look` calls, before `releaseLook`, ease 0.45 toward the new fraction on each `step`. `releaseLook` clears the smoothed point so the next `look` snaps.

Dwell:

1. The entry step is the `step` that first sees this button as the hit after a miss, a different button, a pointer cancel, or a stale sample. `dwellStart` is that `now`. Fill is 0. Nothing commits on the entry step.
2. Fill is `(now - dwellStart) / dwellMs`, clamped to 0..1. At 200 ms the fill is 200/650 on a card, not 0. There is no quiet onset.
3. When `now - dwellStart >= dwellMs`, the writer fires once. 649 ms on a card does not fire. 650 ms does. 1299 ms on a menu button does not fire. 1300 ms does. The named checks use 640 and 660 for play, and 600 and 1300 for menu.
4. After the fire, the fill stays 1 until the pointer leaves. Staying does not fire again.
5. Leaving before the fire discards the visit. A finished fire is not undone by leaving.

`look` during the check stamps freshness from the `step` clock. Each `step` while a test look is held and the internal `refreshHold` flag is true sets the sample time to that `now`. `look` sets `refreshHold` true. The stale case sets it false. The flag is not on the window hook.

A sample 500 ms old or older hides the ring, forces `dwell()` to 0, and does not commit. It does not clear a filled blank and does not change the screen. A pointer down does the same to the in-progress dwell only: the ring may still sit on the look, and the fill stays 0. `release` does not fire the button. A new visit starts only on a later step after the pointer is up.

One pointer at a time. A second contact is ignored until the first ends. Pressing on one button and releasing on another does not invent a drag commit. The click, if the browser fires one, belongs to the button that receives it. `tap` is that writer with no wait.

### Socket

The URL is the literal `ws://127.0.0.1:7081`. It is not built from `location`, a query value, or a message. If `location.protocol` is `https:`, do not construct a socket and do not fall back to another host. If the socket is down, hide the ring, retry every 2000 ms unless checking or suppressed, and let mouse and touch play the whole game.

On every open, send the nine bytes `1, 4, 0, 0, 0, 0, 5, 0, 0`. `binaryType` is `"arraybuffer"`. Framing is a type byte, a little-endian unsigned 32-bit length, then the payload. A length above 1048576 resets the buffer.

- Type 1 with at least 56 bytes is a sample. The mask is the little-endian uint32 at offset 0. Bit 64 must be set. If bit 2 or bit 3 of the mask is set (`mask & 12`), read the left validity from offset 8 when bit 2 is set, otherwise treat it as 4, and the right validity from offset 12 when bit 3 is set, otherwise 4. If both of those validities are not 0, drop the sample. X is the little-endian float64 at offset 40. Y is the float64 at offset 48. Both must be finite and `>= 0`.
- Type 4 with length 0 clears the homography. A non-empty type 4 shorter than 72 bytes is ignored. A type 4 of at least 72 bytes is nine little-endian float64 values. If any is not finite, ignore the message. Apply `x' = (h0 x + h1 y + h2) / w` and `y' = (h3 x + h4 y + h5) / w`, where `w = h6 x + h7 y + h8`. If `abs(w) < 1e-9`, use the raw point. Then multiply by `innerWidth` and `innerHeight`.

Samples and the homography stay in memory. They are not logged, not put in `state()`, and not written into the page.

Reconnect uses `setTimeout` of 2000 ms and only if the check flag is absent. The check itself arms no timer. No `setTimeout` string is part of the check.

### Ring paint

54 px, centered on the smoothed point, `z-index: 60`, `pointer-events: none`. Green fill `#2f8a3e` over `rgba(244, 240, 216, 0.45)`, drawn as a conic gradient on the custom property `--p`. A 3 px cream halo may ring it. Shown only while the sample is fresher than 500 ms. Off a live target, `--p` is `0%`. The target does not grow.

### Card layout

`targetsAt` and the live painter share one layout function. Page pad for chrome is 8 px. Cards sit at least 16 px in from the screen edge. Gap between cards is 24 px when the card region can spare it, otherwise 16 px. Hit boxes match the painted buttons. Do not inflate them.

Play top is 140 px. The Done band is 72 px tall with 8 px below it and 8 px above it, and only on ask. The stem sits under the play top. Give the stem 96 px. If that would push a card's short side under 112 px, give the stem 64 px. Cards take the rest of the region. Never give a card a short side under 72 px. If a pretend width is under 320 px, still return boxes whose short sides are at least 72 px, even if they overflow, and the size case fails that viewport instead of shrinking.

- Two cards: one column, full region width, equal heights.
- Three cards: one column, full region width, equal heights. Do not put three in a row under 480 px wide. At 480 px and wider, one column is still correct.
- Four cards: a 2 by 2. Never one row of four at 390 px. Width of each card at 390 px is `(390 - 32 - 16) / 2 = 171`. Use a 16 px gutter. Height shares the card region equally. On the 390 by 844 and 390 by 700 layouts the short side stays at least 112 px. Cap a card's width at 360 px and center the group.

`#done` is at least 120 by 72 px, centered in the Done band. Menu buttons on the menu card are at least 76 px tall and as wide as the card, which is `min(460, w - 24)`.

`centers()` returns `{ targets: { id: box } }` for every visible button. `box` is `{ x, y, nx, ny, w, h, left, top }` with `x, y` the center in pixels and `nx, ny` that center divided by the live window size. Card ids are `card-0` in `cards[0]` order through `card-3`. Also include `mute`, `full`, `back`, `lets-go`, `tiny`, `little`, `shop`, `rush`, `done`, and `another` when they are visible.

`targetsAt(w, h)` returns `{ targets: [ { id, x, y, w, h }, ... ], ok }`. `ok` is false when `w < 320`. The boxes use `w` and `h`, not the live window.

### Cooldown

On a wrong `answer`, set `cooled` to that value and remember the current gaze visit. Clear both when the hit button is no longer that card, when `releaseLook` runs, or when a pointer click visit ends. While cooled, a gaze fire from the same visit does not call `answer` again. The other cards are not cooled. A later visit, after the look has left or after `releaseLook`, may answer that card again, including another wrong answer. A `tap` is a new visit. Do not add a 280 ms cooldown.

## Generator

The functions are pure. The same arguments return the same value. They do not read the clock, the DOM, the query string, the network, or storage. They live in the page script.

`mathCap(band)` returns tiny 4, little 6, shop 8, rush 10.

`mathCardCount(band)` returns tiny 2, little 3, shop 4, rush 4.

`mathMake(band, seed, index, history)` returns one problem, or `null` if `index` is not an integer in `0 .. cap - 1`.

`history` is the list of problems already answered correctly. Each entry is `{ blank }`. The page appends only after a correct card. The function does not mutate `history`. A missing history is `[]`.

`mathAccepts(problem, value)` is true only for the unique integer completion. `value` must be an integer. A quantity `"set"` accepts only `value === a`. A quantity `"more"` accepts only `value === a + 1`. A repeat accepts only `value === 3 * a`. Any other equation substitutes `value` for the null term and accepts only when the integer equation holds. Division accepts only when the dividend is exactly `divisor * quotient`.

A problem has `kind`, `op`, `a`, `b`, `c`, `blank`, `answer`, `cards`, `layout`, `repeats`, and `range`.

### RNG

`makeRng(seed)` stores `seed >>> 0`. `rngUint(rng)` replaces that state and returns a uint32. Use `Math.imul`. `>>>` is the unsigned shift. `xor` is `^`. `or` is `|`. Do not use BigInt.

1. `state = seed >>> 0` on the first call, otherwise the stored state.
2. `state = (state + 0x6D2B79F5) >>> 0`. Store this value.
3. `t = imul(state xor (state >>> 15), state or 1)`.
4. `t = t xor (t + imul(t xor (t >>> 7), t or 61))`.
5. Return `(t xor (t >>> 14)) >>> 0`.

`rngInt(rng, lo, hi)` is `lo + (rngUint(rng) % (hi - lo + 1))`, both ends included.

`rngShuffle(rng, items)` copies the array and runs Fisher-Yates from the last index down to 1: swap `i` with `rngInt(rng, 0, i)`.

Seed 1 must yield these first four uints, in order: `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`.

`playSeed(clockMs, counter)` is `imul( (clockMs >>> 0) xor imul(counter >>> 0, 0x9E3779B9), 0x85EBCA6B ) >>> 0`. `playSeed(1, 1)` is `0x3B230FE8`. The pure generators never call `playSeed` or `Date.now`.

`choiceSeed(seed, index)` is `imul(seed xor (index + 1), 0x9E3779B1) >>> 0`. The rng for a problem is `makeRng(choiceSeed(seed, index))`, so each index is stable. Operand picks and the card shuffle share that one stream, in the order below.

### Schedule

The index decides the shape. The rng decides only the operands and the card order. History changes Little Pals index 3 and index 5, and nothing else.

**Tiny.** `kind` `"quantity"`, `layout` `"pictures"`, `op` null, `b` null, `c` null, `repeats` 1, `range` `[1, 3]`. Even index: `blank` `"set"`, `a = rngInt(rng, 1, 3)`, `answer = a`. Odd index: `blank` `"more"`, `a = rngInt(rng, 1, 2)`, `answer = a + 1`. Then build cards. Cap 4. The next call returns null.

**Little.** `layout` `"right"`. `range` `[0, 5]`. `blank` is never `"start"`. A part is only `a + □ = c`. It is never subtraction and never the first slot.

| Index | Shape when history has no `"result"` | Shape when history has a `"result"` |
| --- | --- | --- |
| 0 | `+`, result | `+`, result |
| 1 | `−`, result | `−`, result |
| 2 | `+`, result | `+`, result |
| 3 | `+`, result | `+`, part |
| 4 | `−`, result | `−`, result |
| 5 | `−`, result | `+`, part |

**Shop.** One sitting walks this table. Families are not interleaved inside a block. `op` is never `"×"` or `"÷"`. There is no Harder control and no Again control.

| Index | Blank | Range | Op | Layout | Notes |
| --- | --- | --- | --- | --- | --- |
| 0 | result | 0..10 | `+` | right | |
| 1 | result | 0..10 | `−` | right | |
| 2 | part | 0..10 | `+` | right | `a + □ = c` |
| 3 | part | 0..10 | `−` | right | `a − □ = c` |
| 4 | start | 0..10 | `+` | left | `c = □ + b` |
| 5 | start | 0..10 | `−` | left | `c = □ − b` |
| 6 | result | 0..20 | `+` | right | |
| 7 | result | 0..20 | `+` | right | `repeats` 3, the line `k + k + k = □` |

**Rush.** Addition and subtraction use 0..20. Factors are one digit. The early times and divide problems use set A only. Division is the same three-term layout, not a new screen.

| Index | Op | Blank | Pool | Layout |
| --- | --- | --- | --- | --- |
| 0 | `+` | result | 0..20 | right |
| 1 | `−` | result | 0..20 | right |
| 2 | `×` | result | set A | right |
| 3 | `÷` | result | set A | right |
| 4 | `×` | part | set B | right |
| 5 | `÷` | part | set B | right |
| 6 | `×` | start | set C | left |
| 7 | `÷` | start | set C | left |
| 8 | `+` | part | 0..20 | right |
| 9 | `−` | start | 0..20 | left |

Factor sets, ascending: A is `1, 2, 5, 10`. B is `1, 2, 3, 4, 5, 10`. C is `1, 2, 3, 4, 5, 6, 7, 8, 9, 10`.

### Operand pools

`S` is 5 for Little Pals, and 10 or 20 where the schedule says. Build every legal pair with nested loops, then sort by the first component, then the second, ascending. Choose `rngInt(rng, 0, pool.length - 1)`. Pools below are non-empty. An empty pool is a bug. Do not invent a negative. Do not consume rng while building or sorting the pool. The first rng call on an equation problem is the pool index. Tiny and the repeat problem use the rng calls named in their rows instead.

- **Add result.** Pairs `(a, b)` with `a, b >= 0`, `a + b <= S`, not both 0. `c` is null. `answer = a + b`.
- **Subtract result.** Pairs `(a, b)` with `0 <= b <= a <= S`, not both 0. `c` is null. `answer = a - b`.
- **Add part.** Pairs `(a, c)` with `0 <= a <= c <= S`, `answer = c - a`, `sum = a + c` inside the card range, and `sum !== answer`. `b` is null. Equation `a + □ = c`.
- **Subtract part.** Shop only. Pairs `(a, c)` with `0 <= c <= a <= S`, `answer = a - c`, `sum = a + c` inside the card range, and `sum !== answer`. `b` is null. Equation `a − □ = c`.
- **Add start.** Pairs `(b, c)` with `0 <= b <= c <= S`, not both 0. `a` is null. `answer = c - b`. Left layout prints `c = □ + b`.
- **Subtract start.** Pairs `(b, c)` with `b, c >= 0` and `answer = b + c <= S`, not both 0. `a` is null. Left layout prints `c = □ − b`, which is the same relation as `□ − b = c`.
- **Times result.** Pairs `(f, g)` from the factor set, both orders, so `(1, 2)` and `(2, 1)` are both present. `a = f`, `b = g`, `c` null, `answer = f * g`. Card range `[1, 100]`.
- **Times part.** `a × □ = c`. Pairs `(f, g)` from the set with `answer = g`, `c = f * g`, and `sum = f + c` inside `[1, 10]` and not equal to `g`. Card range `[1, 10]`.
- **Times start.** `□ × b = c`. Pairs `(f, g)` from the set. `b = g`, `c = f * g`, `answer = f`. Card range `[1, 10]`. Left layout prints `c = □ × b`.
- **Divide result.** `a ÷ b = □`. Same pairs as times result. `a = f * g`, `b = f`, `answer = g`. Card range `[1, 10]`.
- **Divide part.** `a ÷ □ = c`. `a = f * g`, `c = g`, `answer = f`, and `sum = a + c` inside `[1, 10]` and not equal to `f`. Card range `[1, 10]`.
- **Divide start.** `□ ÷ b = c`. `b = f`, `c = g`, `answer = f * g`. Card range `[1, 100]`. Left layout prints `c = □ ÷ b`.
- **Repeat.** Shop index 7 only. `k = rngInt(rng, 1, 6)`, `a = b = k`, `c` null, `answer = 3k`, `repeats` 3, `blank` `"result"`, `op` `"+"`, `layout` `"right"`, range `[0, 20]`. No pool index call.

No zero in `×` or `÷`. The factor sets already start at 1.

### Cards

Card range is the problem's `range`. Build the list in this order, then `rngShuffle` it on the same rng.

1. Push `answer`.
2. Push the neighbor: `answer + 1` if that integer is inside the range, otherwise `answer - 1`. One of those is always inside the band range.
3. If `blank` is `"part"` and `op` is `"+"`, `"-"`, `"×"`, or `"÷"`, push the sum of the two visible numbers when it is inside the range and not already in the list. The two visible numbers are the two non-null terms among `a`, `b`, and `c`. The pools make that sum legal for every part blank, so the card is present after this step, either as its own push or because it already equals the answer or the neighbor.
4. Let `pool` be the other integers in range, ascending, excluding numbers already in the list. If the list is still shorter than the band's card count, take one `rngInt(rng, 0, pool.length - 1)` as the start index. Walk forward, wrapping to 0 at most once, and push each value that is not already in the list. Then stop. Do not wrap a second time. Do not call `rngInt` again for the filler. An empty filler pool is a bug.

Then shuffle. Every card is an integer in range. Cards are distinct. Negatives never appear. Tiny cards are only `1`, `2`, or `3`. A start blank does not get a required sum card. A result blank does not get a required sum card. For `10 × 5 = □` the sum 15 is not required.

Rng order on one problem: the operand `rngInt` (pool index, tiny bound, or repeat `k`), then at most one filler `rngInt`, then the shuffle's `rngInt` calls. Skip the filler call when the list is already long enough. Tiny's two cards are answer and neighbor, so tiny skips the filler call.

### Seed 11 trace

These are the required `mathMake(band, 11, index, history)` values when history starts empty and each problem's `blank` is appended after a correct accept. `cards` is the shuffled order. Minus in `op` is ASCII `-` in the object.

**Tiny**

| Index | a | blank | answer | cards |
| --- | --- | --- | --- | --- |
| 0 | 2 | set | 2 | 3, 2 |
| 1 | 1 | more | 2 | 3, 2 |
| 2 | 1 | set | 1 | 1, 2 |
| 3 | 1 | more | 2 | 3, 2 |

**Little, history grown**

| Index | Printed, if it were right-layout fields | layout | blank | op | answer | cards |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | `0 + 2 = □` | right | result | `+` | 2 | 4, 2, 3 |
| 1 | `2 − 2 = □` | right | result | `-` | 0 | 0, 1, 4 |
| 2 | `0 + 3 = □` | right | result | `+` | 3 | 5, 3, 4 |
| 3 | `2 + □ = 2` | right | part | `+` | 0 | 4, 0, 1 |
| 4 | `4 − 4 = □` | right | result | `-` | 0 | 0, 1, 3 |
| 5 | `2 + □ = 3` | right | part | `+` | 1 | 5, 1, 2 |

Little index 3 with history `[]` is a result, not a part: `0 + 1 = □`, answer 1, cards `1, 2, 0`. Little index 5 with history `[]` is a subtract result: `3 − 0 = □`, answer 3, cards `3, 1, 4`. Both layouts are right.

**Shop**

| Index | Fields | Printed | answer | cards | repeats | range |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | a 2, b 1, c null | `2 + 1 = □` | 3 | 3, 2, 4, 5 | 1 | 0..10 |
| 1 | a 6, b 4, c null | `6 − 4 = □` | 2 | 2, 4, 3, 1 | 1 | 0..10 |
| 2 | a 4, b null, c 5 | `4 + □ = 5` | 1 | 5, 2, 9, 1 | 1 | 0..10 |
| 3 | a 7, b null, c 2 | `7 − □ = 2` | 5 | 6, 0, 9, 5 | 1 | 0..10 |
| 4 | a null, b 2, c 10 | `10 = □ + 2` | 8 | 9, 8, 0, 10 | 1 | 0..10 |
| 5 | a null, b 0, c 1 | `1 = □ − 0` | 1 | 10, 9, 2, 1 | 1 | 0..10 |
| 6 | a 6, b 6, c null | `6 + 6 = □` | 12 | 13, 14, 11, 12 | 1 | 0..20 |
| 7 | a 4, b 4, c null | `4 + 4 + 4 = □` | 12 | 12, 8, 9, 13 | 3 | 0..20 |

**Rush**

| Index | Fields | Printed | answer | cards | range |
| --- | --- | --- | --- | --- | --- |
| 0 | a 8, b 2, c null | `8 + 2 = □` | 10 | 10, 18, 11, 19 | 0..20 |
| 1 | a 10, b 0, c null | `10 − 0 = □` | 10 | 10, 6, 11, 5 | 0..20 |
| 2 | a 10, b 5, c null | `10 × 5 = □` | 50 | 81, 51, 80, 50 | 1..100 |
| 3 | a 1, b 1, c null | `1 ÷ 1 = □` | 1 | 2, 4, 3, 1 | 1..10 |
| 4 | a 1, b null, c 3 | `1 × □ = 3` | 3 | 4, 3, 9, 8 | 1..10 |
| 5 | a 1, b null, c 1 | `1 ÷ □ = 1` | 1 | 9, 8, 2, 1 | 1..10 |
| 6 | a null, b 7, c 49 | `49 = □ × 7` | 7 | 8, 4, 3, 7 | 1..10 |
| 7 | a null, b 1, c 8 | `8 = □ ÷ 1` | 8 | 8, 19, 20, 9 | 1..100 |
| 8 | a 1, b null, c 12 | `1 + □ = 12` | 11 | 12, 13, 11, 3 | 0..20 |
| 9 | a null, b 6, c 14 | `14 = □ − 6` | 20 | 20, 15, 19, 16 | 0..20 |

Rush index 2 does not require a 15 card. The 15 would be the sum of the two visible factors, and this blank is a result.

## Self-check cases

`selfCheck` runs the family cases, then the math cases, then `engineSelfCheck`, and concatenates the failures. `ok` is true only when the array is empty. Cases call `step` directly. They do not open a socket and do not arm a timer. Assertions read `state()`, `centers()`, `targetsAt`, `dwell`, and `eyes`, plus the closure's current problem when the answer must be known. They do not read screenshots and they do not wait on a frame.

Helper for a snapped visit: `releaseLook()`, `look(nx, ny)`, `step(t)`. That step is the entry. It does not commit. `step(t + 640)` and `step(t + 660)` are measured from that entry. Start the clock at 10000. Call `seed(11)` before any `start` in a case that walks a sitting, so the trace above is what the page shows.

At the start of the check, `checking` is true, reconnect is suppressed, and the socket is null. Snapshot `localStorage` length and the value of `pop-pals-v1` before the later cases.

### Family

1. `family-guards`. At the start, `checking` is true, reconnect is suppressed, and the socket is null.
2. `family-no-storage`. After the later cases, `localStorage` has no new key, `pop-pals-v1` is unchanged, and the page never called `setItem` or `removeItem`.
3. `family-play-dwell`. On a Tiny Tots card, entry then 640 ms leaves `filled` null and `phase` `"ask"`. Entry then 660 ms on the correct card sets `filled` to the answer and `phase` to `"show"`. `tap` on the correct card of a fresh problem does the same.
4. `family-menu-dwell`. On Let's go, entry then 600 ms leaves `screen` `"menu"`. Entry then 1300 ms sets `screen` to `"ask"`. On Back, entry then 600 ms stays `"ask"`. Entry then 1300 ms sets `screen` to `"menu"`.
5. `family-pointer-blocks`. `hold` on the correct card, then a look and a 660 ms step, leaves `filled` null. `release`, then a fresh 660 ms look, fills it.
6. `family-stale`. With `refreshHold` false, a `step` at least 500 ms past the last fresh stamp leaves `dwell()` at 0 and does not fill. Set `refreshHold` true again afterward.
7. `family-target-size`. On ask for every band, `targetsAt(390, 844)` and `targetsAt(1024, 768)` report a short side of at least 72 for every target, and every card's short side is at least 112. `targetsAt(300, 600)` fails this case because the width is under 320. It does not report a short side under 72.
8. `family-eyes`. `eyes()` has only the boolean keys `on` and `live`. A fresh test look sets both true while the socket is null. `#eyes` is shown and its text is "Eyes".
9. `family-mute`. The mute button's visible text is empty. It contains two inline SVGs. The visible text does not contain Mute or Sound. `tap` sets `state().muted` true. A second `tap` sets it false.
10. `family-query-inert`. The junk flag's value is not in the document text or in any attribute. `screen` is still `"menu"`. The socket is null. This is asserted before any `start`.
11. `family-plain-state`. `JSON.stringify(state())` works, and the object contains `level`, `screen`, `banner`, and `muted`.

### Math

1. `tiny-no-symbols`. `start("tiny")` leaves two cards, each in 1..3, `kind` `"quantity"`, `op` null, `layout` `"pictures"`. The text of `#play` and `#banner` contains no `+`, `−`, `-`, `=`, `×`, `÷`, and no digit.
2. `tiny-cap`. Four correct answers, each followed by a step of 700 ms from the commit, land on `"rest"`. The rest card has no grade. A further `answer` does not change `screen` or `index`.
3. `little-result-first`. With seed 11, problems 0, 1, and 2 are `"result"`. Problem 3 is `"part"` and `op` is `"+"`. No index is `"start"`. Every completed total is at most 5. There are three cards. A separate `mathMake("little", 11, 3, [])` is `"result"`, not `"part"`.
4. `little-no-left-total`. Every Little Pals problem has `layout` `"right"`. The painted order is the left term, the operator, the right term, `=`, then the result. It is never a total, then `=`, then an expression.
5. `shop-order`. Seed 11 matches the shop trace, including left layout on indexes 4 and 5, range 0..20 on indexes 6 and 7, and index 7 `repeats` 3 with `answer === 3 * a`. `op` is never `"×"` or `"÷"`. No card is above 20. Result-within-10 is indexes 0 and 1 only, part is indexes 2 and 3 only, and start is indexes 4 and 5 only.
6. `rush-factors`. Seed 11 index 2 has both printed factors in set A. Index 3 has the divisor and the quotient in set A. Index 4 uses set B for the printed factor and the missing factor. Index 6 uses set C for both factors. The printed line matches the rush trace, including left layout on indexes 6, 7, and 9.
7. `distractor-sum`. On every part blank in the seed-11 sittings, the cards include the answer once and include the sum of the two visible numbers, every card is inside `range`, and no other card makes `mathAccepts` true.
8. `wrong-unchanged`. A wrong `answer` leaves the terms and an empty blank. `state()` has none of the forbidden keys. The banner does not contain fail, wrong, or miss. `screen` stays `"ask"`.
9. `cooldown-gaze`. After that wrong commit, another 660 ms step without `releaseLook` and without leaving the card does not change `filled` or `phase`. `releaseLook`, then a new 660 ms look, may commit again and still leaves the problem up when the card is wrong. While the first card is cooled, `answer` of a different card still runs.
10. `done-slow`. A 600 ms look on Done does not change the screen. A 1300 ms look calls `go("rest")`. Rest has no grade.
11. `no-pad`. `targetsAt` does not list a ten-key. The ask screen has 2, 3, or 4 card buttons and no `input` or `textarea`.
12. `no-speech`. After a Tiny Tots sitting, `state()` has no speech field. The page never speaks.
13. `pointer-shares`. `tap` on the correct card sets the same `filled` value a 660 ms look sets.

### engineSelfCheck

`engineSelfCheck()` returns `{ ok, failures }`. It does not call `Date.now`, `setTimeout`, `setInterval`, `requestAnimationFrame`, `fetch`, `WebSocket`, or the speech API. It does not read the DOM. It does not generate the other four games.

1. `rng-seed-1`. Four `rngUint` calls on `makeRng(1)` match `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`.
2. `play-seed`. `playSeed(1, 1)` is `0x3B230FE8`.
3. `rng-int`. Twenty `rngInt` calls on a fresh `makeRng(1)` with range `0..3` each land inside 0..3.
4. `math-unique`. For every band, seed 11, index `0 .. cap - 1`, history grown only by correct blanks: exactly one integer in `range` makes `mathAccepts` true, that integer is `answer`, `cards` contains it once, no other card accepts, the length matches `mathCardCount`, and every card is an integer inside `range`.
5. `math-tiny`. `kind` is `"quantity"`, `layout` is `"pictures"`, `op` is null, values are 1..3, and index 4 returns null. The four problems match the tiny trace.
6. `math-little-empty`. With history `[]`, indexes 0, 1, 2, and 3 are `"result"`. Index 3 matches the empty probe above. No index is `"start"`. `layout` is `"right"`. Every total is at most 5. Three cards.
7. `math-little-part`. With history `[{ blank: "result" }]`, index 3 is `"part"`, `op` is `"+"`, and one card is the sum of the two visible numbers. The grown trace matches.
8. `math-shop`. The shop trace matches. `op` is never `"×"` or `"÷"`. No card is above 20. Index 7 has `repeats` 3 and `answer === 3 * a`.
9. `math-rush`. The rush trace matches. Indexes 2 and 3 use set A as above. Index 4 uses set B. Index 5 uses set B for the divisor and the quotient. Index 6 uses set C. Index 7 uses set C for the divisor and the quotient.
10. `math-part-sum`. Every part blank in those sittings includes the sum of the two visible numbers.
11. `math-cap`. `mathMake` at `index === cap` returns null for every band.

## README paragraph

Add this bullet to the Play list, after Saddle Up!:

- [Math Practice](https://calledtoconstruct.github.io/games/math-practice/)

Put this section after the Saddle Up! section and before Add a game.

## Math Practice

A quiet page of one hole for ages 2 and up. The child looks at the picture or the equation, then looks at the card that fills the hole. A miss stays on the page. Nothing keeps score, and nothing is timed.

Tiny Tots match apples, one or two or three, with no plus or minus on the page. Little Pals add and take away, and the totals stay inside 5. Shopkeeper uses plus and minus up to 20, and the hole can be the end, the middle, or the start. Rush adds times and divide, still one hole, with the early times using 1, 2, 5, and 10.

Open `math-practice/index.html` in a browser, or play [Math Practice](https://calledtoconstruct.github.io/games/math-practice/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an answer card for 650 ms, until the ring fills. Done and Back to Menu take twice that, so reading one does not press it. Mouse and touch work on the same cards. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Key Decisions

- The dwell is 650 ms on a card and 1300 ms on every other button. The ring fill is `elapsed / dwellMs` from the entry step. There is no 200 ms onset and no second timer. 640 ms does not commit. 660 ms does. 650 ms does. Menu commit is at 1300 ms, not at 1299 ms.
- `LEAVE_MS` is 280 and is only a spelled constant. Leaving a card cancels the open visit on the next step. A wrong card's silence is a writer flag until the look leaves, not a 280 ms grace.
- The engine schedule, card rule, and seed-11 trace win where an age list or a gaze size note differs. Shopkeeper and Rush do not walk a Harder ladder. One sitting is the index table. Little Pals parts are only `a + □ = c`, and only after a result is in history. Left layout is printed for every schedule row marked left, including Shopkeeper start and Rush indexes 6, 7, and 9.
- Card counts stay 2, 3, and 4. A short window does not drop a card. Under 320 px wide, the size case fails instead of shrinking below 72 px. Checked card short sides stay at least 112 px at 390 by 700, 390 by 844, and 1024 by 768.
- The neighbor is `answer + 1` when it is in range, otherwise `answer - 1`. The sum of the two visible numbers is required only when `blank` is `"part"`. It is not required on a product result. `10 × 5` therefore does not have to offer 15. Tiny Tots still show the age pictures: matching trays for `"set"`, and one outline for `"more"`. The fixed four-row apple script and the fixed first-sitting equations are not the generator.
- A miss does not reset the problem, does not score, does not buzz, and does not say wrong. The show beat is 700 ms of `step` time, then `next`. Caps are ceilings. Done can rest on the first problem.
- This file does not speak, does not store, and does not load a remote asset. `?selfcheck` is synchronous and does not open the socket.

## Out of scope

- A keypad, handwriting, a drag, a compare problem, two steps, parentheses, or a remainder.
- Speech, recorded audio, a buzzer, a score, a star, a life, a mistake tally, or a visible clock.
- Harder, Again, Undo, Hint, a subject menu, or a fifth band.
- `localStorage`, cookies, fetch, remote pictures, remote fonts, and a second script.
- An onset dwell, a 280 ms leave grace, blink or wink, and a shared gaze file.
- The other four games' generators, and any edit to their folders or to the README from this spec. The README section above is the text to paste when the page is added.

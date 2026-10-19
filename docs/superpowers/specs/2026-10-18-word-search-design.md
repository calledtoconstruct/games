# Word Search

**Date.** 2026-10-18. Size revised 2026-10-19. Marks revised 2026-10-19.

**Status.** Build spec. One implementer can ship the page from this file alone. The 2026-10-19 Size section replaces every earlier line that says the page may scroll, that a short side stays at least 72 px, or that Previous and Next page the grid. The Marks section replaces the found-cell gold fill and the letter-button border.

**Folder.** `word-search/index.html` in `/home/joseph/code/games`.

**Window.** `window.wordSearch`, assigned as soon as the script runs, before the query is handled.

**Title.** Word Search.

This page copies the Saddle Up! gaze client. It does not import it. There is no `gaze.js`, no second script, no module, and no asset folder. The license stays the repo MIT license. Copyright 2026 Joseph Woolley. The HTML file does not add a second license or a different copyright.

## Player loop

One child, ages 2 and up, one device. Mouse, touch, and gaze do the same acts. A tap or a click is that act with no wait. A look commits at 650 ms on a play button and at 1300 ms on every other button. Nothing in the game is a drag.

1. The menu shows the title, the line "Look at the first letter and the last letter.", the line "Ages 2 and up.", and four band buttons. Tiny Tots starts highlighted. Let's go starts that band and opens the group screen. Starting does not open the socket.
2. The group screen lists only that band's groups, in the order below. A finished look on a group places it from a new seed and opens the puzzle. Playing the same group again places it again. The bank order does not change. The letters do.
3. The puzzle is a grid of letter buttons and a bank under the grid. The child dwells one cell, then dwells the other end of a word. Either end may be first. When those two cells are exactly the ends of one word that is not found yet, the whole word locks, the bank chip is marked, and the anchor clears. The child does not have to look at the middle letters.
4. A finished look that is not that pair does not buzz, does not score, and does not drop words already found. The cell that was just committed becomes the anchor.
5. A finished look on a bank chip says that word aloud and, on Tiny Tots and Little Pals, shows its picture. It does not mark the word found.
6. When every word is found, the page opens the rest card. The banner is "All the words." There is no grade, no count, no time, and no rank.
7. Another, when it is offered, goes back to the group screen for the same band. It does not pick a group by itself and it does not roll a new band. The child may play one more puzzle. After that second puzzle, the rest card has no Another. Back to Menu is always there. Leaving early is not a miss.

There is no countdown. There is no mistake tally. A wrong look never says fail, wrong, no, miss, or lose.

## Bands

Parent names and button ids stay Tiny Tots `tiny`, Little Pals `little`, Shopkeeper `shop`, and Rush `rush`. Do not add a fifth band. Do not rename Rush.

Grid size and the overlap rule are the engine rules, not the age table. The age table's Tiny Tots grid was 3 rows by 4 columns. The age table's Little Pals grid was 4 rows by 5 columns. Both are rejected. Tiny Tots is 3 rows by 5 columns. Little Pals is 5 rows by 4 columns. Shopkeeper is 6 by 6 and does not grow. Rush starts at 6 by 6 and may grow once, to 7 rows by 6 columns, and only then. Do not add a column past 6.

| Band | Ages | Grid | Words | Letters | Directions that may be placed | Groups | Puzzles in one start |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Tiny Tots | about 2–3 | 3 rows × 5 columns, fixed | 3 | 3 | `right` | 4 | 2 |
| Little Pals | about 4–5 | 5 rows × 4 columns, fixed | 4 or 5 | 3 or 4 | `right` | 6 | 2 |
| Shopkeeper | about 6–8 | 6 × 6, fixed | 6, 7, or 8 | 4 or 5 | `right`, `down` | 8 | 2 |
| Rush | about 9 and up | 6 × 6, then once 7 rows × 6 columns | 8 | 4 through 7 | `right`, `down`, `downRight`, `downLeft` | 8 | 2 |

The age line "1, then one optional Another, then stop" and the age line "2" are the same ceiling. `start` sets `puzzlesDone` to 0. Each finished puzzle adds 1 and opens rest. Another is drawn only when `puzzlesDone` is 1. The second rest does not draw it. Shopkeeper and Rush do not auto-open a second puzzle and do not skip rest.

Blurbs under the band buttons, one line each:

- Tiny Tots. Three words, across.
- Little Pals. A few more words, still across.
- Shopkeeper. Across and down.
- Rush. Across, down, and slanted.

Pictures are canvas drawings on Tiny Tots and Little Pals chips only. Shopkeeper and Rush chips are the uppercase word and nothing else. No picture is a downloaded file. No drawing contains a letter, a logo, or a face with a smile.

Cells are square buttons. Spare pixels make the cells larger. They never add a column. The bank sits under the grid, not beside it. Letters in the grid are uppercase. The bank lists words in the order written in this spec, not in the order they were placed.

Rush alone may turn on four more placement directions: `left`, `up`, `upLeft`, `upRight`. The Backward button on the Rush group screen does that. It is off unless the child turns it on, or the page was opened with the `harder` flag outside the self-check. The value of the flag is ignored. Backward is not a band. It does nothing on Tiny Tots, Little Pals, or Shopkeeper. The span scan is all eight directions whether Backward is on or not.

Overlap means at least one shared cell between two answers. Do not reject a grid that already has one span per word and a shared cell just because an overlap ratio is under 1.15. Do not reject for how full the grid is. The six Little Pals play lists are the exception in Key Decisions: those strings cannot share a cell on a 4-column right-only row, so a one-span grid is kept anyway.

## All 26 groups

`group(id)` looks up `id` in the current band only. The same id on two bands is two lists. An id from another band does nothing. The button label is the name. The id is never shown.

A stored fallback is the grid `useFallback` loads, and it is also the grid seed 1 generates for that group with `fallbackUsed` false. Rows are top to bottom. Column 0 is the left. Every fallback below already has one eight-direction span per answer and zero spans of each denylist token.

Where the age lock printed a fallback, that drawing was the wrong size. The rows in this spec replace it. Body and Farm use the engine fixture rows for those same words. The other groups had no fixture with their words. Their rows are the seed-1 grids from the generator in this spec.

### Tiny Tots

Home band `tiny`. Three words. Pictures on the chips. `requireShare` true.

**1. Body.** Id `body`. Words `EAR`, `ARM`, `EYE`. `EYE` is a palindrome and counts as one span.

The age fallback was 3 rows by 4 columns (`EYEX`, `XKZF`, `EARM`). That size is rejected. Use the engine fixture for these words. Fixture id `body`. Seed 1 and the fallback are:

```
EARME
STRMX
EYEDH
```

`EAR` is row 0, columns 0–2. `ARM` is row 0, columns 1–3. They share two cells. `EYE` is row 2, columns 0–2, direction `right`.

Drawings: ear, side view, peach fill. Bent arm, peach, a simple hand, no face. One eye, white oval, dark iris, no surrounding face.

**2. Farm.** Id `farm`. Words `COW`, `OWL`, `HEN`.

The age fallback was 3 by 4 (`COWL`, `XXKZ`, `HENF`). That size is rejected. The engine fixture with these words is id `pets`, which is not a menu group. Use its rows. Seed 1 and the fallback are:

```
COWLE
STRMX
HENDH
```

`COW` is row 0, columns 0–2. `OWL` is row 0, columns 1–3. They share two cells. `HEN` is row 2, columns 0–2.

Drawings: side cow, white with two black spots, four legs, no brand, no smile. Front owl, brown, two round eyes, a small beak. Side hen, red comb, brown body.

**3. Small animals.** Id `small-animals`. Words `BEE`, `EEL`, `CUB`.

The age fallback was 3 by 4 (`BEEL`, `XXKZ`, `CUBF`). That size is rejected. There is no engine fixture for these three words. Seed 1 and the fallback are:

```
BEELE
STRMX
CUBDH
```

`BEE` is row 0, columns 0–2. `EEL` is row 0, columns 1–3. They share two cells. `CUB` is row 2, columns 0–2.

Drawings: yellow oval, two black stripes, two wings. One long blue eel, a small fin, no smile. Small brown bear, round ears, no clothes, no smile.

**4. Play.** Id `play`. Words `CAP`, `APE`, `BOX`.

The age fallback was 3 by 4 (`XXKZ`, `CAPE`, `BOXF`). That size is rejected. There is no engine fixture for these three words. Seed 1 and the fallback are:

```
CAPEE
STRMX
BOXDH
```

`CAP` is row 0, columns 0–2. `APE` is row 0, columns 1–3. They share two cells. `BOX` is row 2, columns 0–2.

Drawings: blue cap, curved brim, no logo. This is not the Little Pals hat. Side ape, brown, one long arm, no clothes, no smile. Red box, a lid, no label.

### Little Pals

Home band `little`. Pictures on the chips. `requireShare` false. Read Key Decisions before "fixing" these grids. They are 5 rows by 4 columns. No two answers share a cell. Each answer is `right` and has one span. Seed 1 returns `fallbackUsed` false.

The age fallbacks were 4 rows by 5 columns. That size is rejected. None of those six lists is the engine fixture `little-farm`. Do not swap a list for `little-farm`.

**5. Snack.** Id `snack`. Words `BUN`, `NUT`, `PIE`, `CAKE`.

```
SPIE
CAKE
FIRB
NUTM
BUNR
```

Drawings: tan roll. One walnut shape. Whole pie, gold crust. Square cake, pink top, no writing.

**6. Toys.** Id `toys`. Words `NET`, `TOP`, `KITE`, `BALL`.

This is not the engine fixture `toys` (`BAT`, `TOP`, `CAR`). That fixture is not a menu group.

```
KHYK
KITE
BALL
BTOP
HNET
```

Drawings: a hoop with a mesh bag. Spinning top, red and yellow bands. Red diamond kite, short tail. Blue ball, one white stripe, no logo.

**7. Animals.** Id `animals`. Words `PUP`, `PIG`, `CALF`, `LAMB`.

```
KHYK
CALF
LAMB
BPIG
HPUP
```

Drawings: tan puppy, floppy ear, no collar. Side pig, pink, no clothes. Small calf, brown and white. White lamb, black legs.

**8. Clothes.** Id `clothes`. Words `HAT`, `TIE`, `SHOE`, `SOCK`.

```
KHYK
SHOE
SOCK
BTIE
HHAT
```

Drawings: yellow hat, tall crown, wide brim, distinct from the Tiny Tots cap. Red necktie. Side brown shoe. Blue sock with a cuff.

**9. Home.** Id `home`. Words `TUB`, `BED`, `RUG`, `CRIB`.

```
SRUG
CRIB
FIRB
BEDM
TUBR
```

Drawings: white bathtub, side view, dark rim. Bed, brown frame, blue blanket. Red rug, darker border. Crib with side rails, no mobile.

**10. Outside.** Id `outside`. Words `BUD`, `DEW`, `LOG`, `SUN`, `HAY`.

```
PDEW
HHAY
LOGG
XSUN
BUDZ
```

Drawings: closed pink bud, green sepals. One leaf with three blue drops. Brown log, cut end visible. Yellow sun disk, eight short orange rays, no face. Yellow hay bale with two bands.

### Shopkeeper

Home band `shop`. No pictures. `requireShare` true. Directions placed at seed 1 are only `right` and `down`. Grids are 6 by 6. The age lock did not print fallbacks for these groups. The rows are seed 1.

**11. Kitchen.** Id `kitchen`. Words `FORK`, `BOWL`, `OVEN`, `SINK`, `TRAY`, `SPOON`.

```
FVSYPS
OJPHYI
RWOVEN
KBOWLK
LJNKXI
TVTRAY
```

**12. Animals.** Id `animals`. Words `FROG`, `DUCK`, `BEAR`, `WOLF`, `LION`, `GOAT`, `MULE`.

This is not the engine fixture `shop-animals`.

```
OWOLFG
BEARRG
DUCKOG
COMUGO
FMULEA
LIONHT
```

**13. Garden.** Id `garden`. Words `TREE`, `LEAF`, `ROSE`, `VINE`, `SEED`, `SOIL`, `BULB`, `STEM`.

```
VINEGU
IKBULB
SRTREE
OOSEED
ISTEMO
LEAFFH
```

**14. Weather.** Id `weather`. Words `RAIN`, `SNOW`, `WIND`, `HAIL`, `CLOUD`, `STORM`.

```
BYSNOW
LCTBMW
CLOUDI
IRRAIN
ILMIXD
QLHAIL
```

**15. Home.** Id `home`. Words `LAMP`, `DOOR`, `SOFA`, `DESK`, `ROOF`, `GATE`, `WALL`.

```
BRLAMP
OOSTMO
DOORTG
EFFZDA
SWALLT
KKWDEE
```

**16. Ocean.** Id `ocean`. Words `FISH`, `CRAB`, `SAND`, `WAVE`, `REEF`, `BOAT`.

```
YFFISH
PNCRAB
WAVENO
MKBEDA
GHJFQT
FFFYJG
```

**17. School.** Id `school`. Words `BOOK`, `GLUE`, `RULER`, `PAPER`, `CHALK`, `PAINT`.

```
RCPSDB
RHABOH
UAIOGZ
LLNOLG
EKTKUR
RPAPER
```

**18. Roads.** Id `roads`. Words `ROAD`, `CART`, `PATH`, `LANE`, `RAIL`, `PORT`.

```
KQPORT
RGCART
HOPKAL
ROADIA
BITULN
UGHRFE
```

### Rush

Home band `rush`. No pictures. `requireShare` true. Eight words. Seed 1 below is with Backward off. Clothing and Wild animals grow to 7 rows by 6 columns because `SWEATER` and `DOLPHIN` do not fit in 6 by 6. The other six stay 6 by 6 at seed 1. The age lock did not print fallbacks. These rows are seed 1.

**19. Orchard.** Id `orchard`. Words `PEAR`, `PLUM`, `BEET`, `CORN`, `BEAN`, `MINT`, `OLIVE`, `GRAPE`.

```
NMCDBO
CEIEEL
OPANEI
RNEYTV
NGRAPE
PLUMRN
```

**20. Forest.** Id `forest`. Words `PINE`, `MOSS`, `FERN`, `TWIG`, `BARK`, `ROOT`, `MAPLE`, `CEDAR`.

```
BARKRM
RCNFOA
PDESOP
IRSDTL
NROZAE
ETWIGR
```

**21. Sky.** Id `sky`. Words `MIST`, `HAZE`, `DUSK`, `DAWN`, `GUST`, `FROST`, `SLEET`, `VAPOR`.

```
UHDFFF
OVAPOR
MDWZZO
IUNLES
SSLEET
TKGUST
```

**22. Clothing.** Id `clothing`. Words `COAT`, `VEST`, `BELT`, `BOOT`, `HOOD`, `SWEATER`, `SCARF`, `GLOVE`. Seven rows.

```
SBOOTH
WCESGO
ECALFO
ABORTD
TVTAFZ
EVESTY
RUOMLF
```

**23. Tools.** Id `tools`. Words `PEEL`, `MITT`, `ZEST`, `TONGS`, `WHISK`, `LADLE`, `SCOOP`, `SIEVE`.

```
WZESTL
HPEELA
IMITTD
SCOOPL
KSIEVE
TONGST
```

**24. Wild animals.** Id `wild-animals`. Words `DOLPHIN`, `SEAL`, `HARE`, `TOAD`, `NEWT`, `MOTH`, `CRANE`, `HERON`. Seven rows.

```
DHHNCM
ODEERV
LMRWAN
POOTNS
HTNOEE
IHHARE
NPLDGO
```

**25. Places.** Id `places`. Words `PARK`, `FARM`, `POND`, `CAVE`, `HILL`, `LAKE`, `BEACH`, `ISLAND`.

```
ICAVEI
KSPOND
HILLFQ
FBEACH
PARKNQ
HMJEJD
```

**26. Workshop.** Id `workshop`. Words `NAIL`, `BOLT`, `GEAR`, `WOOD`, `BRICK`, `PLANK`, `CLAMP`, `SCREW`.

```
CCLAMP
EBSNBL
GRCAOA
EIRILN
ACELTK
RKWOOD
```

That is 26 groups: 4, then 6, then 8, then 8.

## Screens and API

### Page

One HTML document. No frame. The head holds charset, a viewport meta (`width=device-width, initial-scale=1`), theme color `#f4f0d8`, and the title Word Search. Nothing else is loaded. No remote image, font, style sheet, script, or beacon.

Colors: paper `#f4f0d8`, card `#fffaf0`, ink `#2c2416`, sun-gold `#e2a62e`, found cell `#f3d48a`, preview fill `#d5ead8`, ring green `#2f8a3e`. Font stack: Arial Rounded MT Bold, Trebuchet MS, Segoe UI, sans-serif. Cards are cream. Primary buttons are sun-gold. Text is ink on cream.

Body order:

1. No full-page canvas. Do not add `#view`. Pictures are small canvases inside chip buttons and inside the reveal panel. The grid is buttons, so gaze never hits a canvas.
2. Chrome, always in the tree: `#mute`, `#full`, `#back`, `#eyes`, `#gaze`, `#banner`.
3. Menu card `#screen-menu`.
4. Play host `#play`, which holds the group buttons and the puzzle buttons.
5. Rest card `#screen-rest`.
6. `#selfcheck-result` is not in the authored markup. The loader creates that `pre` only when `selfcheck` is present.

`#mute` matches the chrome side, 72 by 72 when the window allows, two inline speaker SVGs, one hidden at a time. One is a speaker with waves. One is a speaker with a slash. The visible text is empty. The words Mute and Sound are not visible. The accessible name is "Mute sound" or "Turn sound on". `#full` is 72 by 72 and says Fullscreen. `#back` says Back to Menu, dwells 1300 ms, and is hidden on the menu. `#eyes` is not a button. It shows the word Eyes when the socket is open or a test look is held, and it is hidden otherwise. `#banner` is one short sentence, `role="status"`, and it is not a button, not a score, not a timer, and not a rank. `#gaze` is the ring.

`go(screen)` is the only writer of the screen id. It shows one of `menu`, `group`, `puzzle`, and `rest`, and hides the others. Unknown ids do nothing. `go("puzzle")` does nothing when no grid has been placed. Helpers call `go`. They do not assign the screen themselves.

Banner strings, exact:

| When | Banner |
| --- | --- |
| Menu | Look at the first letter and the last letter. |
| Group | Pick a group. |
| Puzzle, no anchor, nothing revealed | Look at the first letter. |
| Puzzle, anchor set, nothing revealed | Look at the last letter. |
| A bank word is revealed | That word, uppercase, and nothing else. |
| Rest | All the words. |

A miss uses the anchor sentence. It does not use fail, wrong, lose, or miss.

### Hook

`window.wordSearch` has these functions. Names match the other games. There is no second parent mode.

- `start(band)` sets the band when `band` is `tiny`, `little`, `shop`, or `rush`. Anything else does nothing. It clears the puzzle, `found`, the anchor, the reveal, `said`, `pulse`, and `puzzlesDone`, then calls `go("group")`. It does not open the socket. It does not clear `harder`. It does not clear `muted`.
- `go(screen)` as above.
- `look(nx, ny)` places a test gaze at window fractions. It does not commit. During the check it stamps freshness from the `step` clock.
- `releaseLook()` drops the test gaze.
- `hold(nx, ny)` is a pointer down. A second hold is ignored until `release`.
- `release()` is the pointer up. It does not fire a button.
- `tap(nx, ny)` runs the writer for the button at that point, with no dwell, and does not leave the pointer down.
- `step(now)` is the only clock.
- `dwell()` returns the ring fill from 0 to 1.
- `eyes()` returns `{ on, live }` and nothing else. No coordinates.
- `centers()` returns `{ id, nx, ny, x, y, w, h }` for every live button. `nx` and `ny` are the center as window fractions.
- `targetsAt(w, h)` returns `{ fit, targets }` for a pretend viewport. Each target is `{ id, x, y, w, h }` in CSS pixels. `fit` is true when every target lies inside that viewport. The function scales the scene. A width under 320 still fits when the boxes are inside.
- `state()` returns plain data. It always includes `level`, `screen`, `banner`, and `muted`. It never includes a DOM node, a gaze coordinate, the socket URL, or the query string.
- `seed(n)` pins the next `group` generate only, as `n >>> 0`. A non-number does nothing. It does not change a puzzle that is already on screen. It is not read from the query. `useFallback` does not consume the pin.
- `selfCheck()` returns `{ ok, failures }`. Each failure is one sentence.

`start`, a band button, and Let's go share one band writer. A play button, `tap`, and a finished look share that button's writer. `render` runs before any of these return, including while the check flag is on and the frame loop is off.

Extra writers:

- `group(id)` places that group when `id` is in the current band's table. An unknown id does nothing and is not written into the page. It uses the pinned seed if there is one, then clears the pin. Otherwise it uses `playSeed`. It calls `go("puzzle")` only when the generator returns `ok`. If the generator returns not ok, the screen stays `group` and the banner stays "Pick a group."
- `anchor(cellId)` is the first-cell writer. The cell id is `row-col`, row 0 at the top, column 0 at the left. An unknown id does nothing. If that cell is already the anchor, the anchor clears. Otherwise the anchor becomes that cell.
- `commit(cellId)` is the second-cell writer. The straight segment between the anchor and this cell, either order, locks a word when that cell set is exactly one unfound answer. The bank chip is marked and the anchor clears. If it is not, `found` stays, the anchor becomes this cell, and nothing is added to state that counts misses. Committing the anchor cell clears the anchor, same as `anchor` on that cell.
- `pressCell(cellId)` is what the cell button, the tap, and the finished look call. No anchor: `anchor`. Same cell as the anchor: clear. Any other cell: `commit`.
- `reveal(wordId)` is the bank writer. `wordId` is the uppercase word. It must be in the current list. It sets `revealed` and `said` to that word. It speaks that word. It does not push it onto `found`. On Tiny Tots and Little Pals it shows the picture. On Shopkeeper and Rush `picture` stays false. An unknown id does nothing and is not inserted into the page.
- `useFallback(id)` loads the stored rows for that id in the current band, scans them, and if they pass, places that grid, sets `fallbackUsed` true, and calls `go("puzzle")`. If they fail or the id is unknown, it does nothing. Play does not call this. The generator calls the same row data only after the attempt cap.
- `backward()` toggles `harder` when `level` is `rush`. On any other band it does nothing. It does not rebuild a puzzle that is already placed. The button exists only on the Rush group screen.

The cell button id is `cell-row-col`. The bank button id is `bank-` plus the word. The group button id is `group-` plus the group id. Let's go is `go`. Another is `another`. Backward is `backward`. There is no Previous button and no Next button.

`state()` also returns:

| Field | Value |
| --- | --- |
| `level` | `tiny`, `little`, `shop`, or `rush`. On the menu, this is the highlighted band. Default `tiny`. |
| `screen` | `menu`, `group`, `puzzle`, or `rest`. |
| `banner` | The sentence above. |
| `muted` | Boolean. |
| `group` | Group id, or null. |
| `rows`, `cols` | Numbers, or 0 off a puzzle. |
| `grid` | Row strings, or `[]`. |
| `anchor` | `row-col`, or null. |
| `found` | Word ids in the order locked. Not a count of misses. |
| `words` | The bank, age order. |
| `directions` | The play set for this puzzle, names only. |
| `placements` | `{ word, row, col, dir }` in placement order. `dir` is the reading direction, which is always in `directions`. |
| `revealed` | The bank word last shown, or null. A cell press clears it. |
| `picture` | True only when a Tiny Tots or Little Pals reveal is showing a drawing. |
| `said` | The last word passed to speech, or null. It stays until the next bank commit replaces it. A cell press does not clear it. |
| `harder` | Boolean. False until Backward or the `harder` flag. |
| `fallbackUsed` | Boolean. |
| `puzzlesDone` | 0, 1, or 2. |
| `cap` | 2. |
| `col0` | Always 0. The full grid is on screen. |
| `pulse` | `row-col` of the idle pulse, or null. |
| `shared` | True when at least two placements share a cell. |

No `miss`, `wrongs`, `score`, `lives`, `rank`, `msLeft`, or gaze coordinate.

### Flags

Deep links run only when `selfcheck` is absent. A flag is present or not. Its value is ignored and is never written into text or an attribute. Unknown flags do nothing. The socket URL is not built from a flag.

Fixed list: `selfcheck`, `play`, `rush`, `harder`.

Outside the check, apply `harder` first (set the boolean). Then if `rush` is present, `start("rush")`. Else if `play` is present, `start("tiny")`. Both band flags: `rush` wins.

`?selfcheck` is synchronous. Handle it before any frame and before any socket. Set `checking` and `suppressReconnect`. Do not apply `play`, `rush`, or `harder`. Call `selfCheck()`. Create `pre#selfcheck-result`. Set its `textContent` to the JSON `{ ok, failures }`. Set `document.title` to `SELFCHECK_OK` or `SELFCHECK_FAIL`. Stop. Do not call `requestAnimationFrame`. Do not construct a WebSocket. Do not arm a timer. If a socket already existed, close it and keep reconnect suppressed. The check drives time only through `step`, from 10000. `performance.now()` does not decide a dwell during the check. No `setTimeout` string and no wait is part of the check.

The harness opens the file with `?selfcheck&zz=http://evil.example/<b>`. Game cases still pass on that URL. The value `http://evil.example/<b>`, the host `evil.example`, and the tag `<b>` are not in any text node or attribute.

### Rest, Another, and finish

The commit that locks the last word increments `puzzlesDone` and calls `go("rest")`. Another's writer calls `go("group")` when `puzzlesDone` is 1. It does not call `start`. At `puzzlesDone` 2 the button is not in the tree, and a direct call does nothing. Back calls `go("menu")` from every screen except the menu, where it is hidden. Back does not increment `puzzlesDone` and does not clear `harder`.

### Sound and speech

The oscillator starts after the first pointer press or the first committed gaze action. Master gain is 0.5 unmuted and 0 muted. Mute does not remove buttons. A locked word plays one sine tone at 440 Hz for 120 ms. A miss plays nothing. There is no buzzer and no recorded file.

Speech is only `speechSynthesis`, and only from `reveal`, and only after that bank dwell or bank tap has committed. The utterance text is exactly that one uppercase word from the current group's list. It is not a sentence, not a banner, not a filler letter, not a query value, and not a denylist token. One utterance at a time. A new bank commit cancels the previous. Leaving the chip before 1300 ms does not speak and does not set `said`. A cell does not speak. A miss does not speak. Group chips, bands, Let's go, Back, Backward, Another, mute, and fullscreen do not speak.

If `speechSynthesis` is missing, `reveal` still sets `said` and `revealed`. The check does not wait for voices. Do not choose a voice URI. Do not read a voice from the query.

`prefers-reduced-motion: reduce` skips the idle pulse animation and any other decorative motion. The commit still happens. `state().pulse` is still set. The tone may still play.

### Storage and network

Do not call `localStorage`, `setItem`, `removeItem`, or cookies. Do not `fetch`. Do not read storage except the self-check spy described below. The WebSocket host is the literal `ws://127.0.0.1:7081`. An `https` page does not open it and does not fall back to another host.

### Threats

- The socket string is a literal. Nothing concatenates a host, a query, or a payload onto it.
- No `innerHTML`, `insertAdjacentHTML`, or `document.write`. Labels, letters, and the banner are authored strings assigned with `textContent`. Gaze bytes are numbers.
- No `eval`, no `new Function`, and no timer that takes a string.
- Speech text is one member of the current word list. Picture ids and filler letters are not spoken.
- Flags are presence checks against the fixed list. The raw search string is never written into text or an attribute. `harder`'s value is not written into the page.
- `state()` and `eyes()` do not expose gaze coordinates. The check does not open the socket. There is no text field.
- `group`, `anchor`, `commit`, and `reveal` ignore unknown ids and do not insert those strings into the page.
- Filler letters are one character from A to Z. The seven denylist tokens are never answers.
- The generator stops at the caps below. A check cannot hang.

## Gaze targets

Copy the socket, the hello, the framing, the homography, and the 0.45 smoothing from `saddle-up/index.html`. Do not copy an onset. The clock below is the whole dwell rule. There is no silent first 200 ms.

### Constants

Spelled this way:

- `GAZE_URL` is `ws://127.0.0.1:7081`
- `PLAY_DWELL_MS` is 650
- `MENU_DWELL_MS` is 1300
- `GAZE_FRESH_MS` is 500
- `LEAVE_MS` is 280

There is no third dwell. There is no onset added before the ring. 640 ms on a play target does not commit. 660 ms does. 600 ms on a menu target does not commit. 1300 ms does.

### Socket

On every open, send the nine bytes 1, 4, 0, 0, 0, 0, 5, 0, 0. Framing is a type byte, a little-endian unsigned 32-bit length, then the payload. Type 1 with at least 56 bytes is a sample. Type 4 with at least 72 bytes is the homography, nine little-endian floats. A non-empty type 4 shorter than 72 bytes is ignored. An empty type 4 clears the homography. A length above 1 MiB resets the buffer. Samples and the homography stay in memory. They are not logged, not put in `state()`, and not written into the page. Reconnect waits 2000 ms, and only if the check flag is absent. Each sample moves 0.45 of the way toward the new point, then the homography is applied when one is stored.

### Ring

One ring, 54 px, centered on the pointer, green fill `#2f8a3e` over paper `rgba(244, 240, 216, 0.45)`, `z-index` 60, `pointer-events: none`. It is shown only while the sample is fresher than 500 ms. A stale look does not commit.

Fill is `elapsed / D` across the whole dwell. At 0 ms the fill is 0. At 650 or 1300 it is 1 and the act fires once. Do not use `(t − 200) / (D − 200)`. Do not keep the ring empty for a hidden first 200 ms. Off a button, the fill is 0 except during the leave grace below. The target does not grow. Staying on the button after the fire does not fire again.

`elapsed` counts only while the look is on that same button. `D` is 650 if the button has `data-play="true"`, and 1300 otherwise.

### Leave grace

`LEAVE_MS` is the grace after the look leaves the button that owns the in-progress dwell. It is not a second dwell and it is not the horse game's faster-boost latch.

- When the hit button changes, keep `dwellStart` and the accumulated on-button time for 280 ms.
- Time off the button does not add to `elapsed`. The ring may follow the pointer. The fill stays at the fraction already earned.
- If the look returns to that same button before 280 ms, `elapsed` continues.
- If `step` reaches the leave time plus 280 ms and the look is still off that button, drop the in-progress dwell and set the fill to 0.
- Leaving does not clear the anchor, `found`, `revealed`, or `said`.
- A sample that `step` ages to 500 ms or more drops the in-progress dwell, sets the fill to 0, and does not clear the anchor. Fresh means age under 500 ms.

### Pointer

Hit testing is `elementFromPoint`, then the nearest `button`. The canvas is not a hit target. There is no geometric walk, no blink, no wink, and no drag that commits a gaze.

One pointer. A second contact is ignored until the first ends. While a pointer is down, gaze dwell does not advance and the ring fill stays at 0. The ring may still sit on the look. A pointer press clears an in-progress gaze dwell. It does not clear the anchor. Releasing the pointer does not fire the button. A press on one cell and a release on another is not a stroke and does not change `found` or the anchor.

A tap fires the same writer as a finished look. Two taps are two commits. They are not a drag.

### Who dwells what

Play, 650 ms, `data-play="true"`:

- A letter cell. Every column is in the tree.

Menu, 1300 ms, no `data-play`:

- A bank chip. This speaks and does not mark the word found.
- A group chip.
- A band button, Let's go, Back to Menu, Backward, Another, mute, and fullscreen.

Not targets: the banner, the Eyes chip, the ring, the reveal picture, and the gutter between cells.

### What a finished look does

- No anchor: that cell becomes the anchor and stays after the look leaves.
- The anchor cell again, after a real leave past the grace or after `releaseLook`, then a new 650 ms: clear the anchor. A second tap on that cell does the same.
- A different cell: if the straight cell set is exactly one unfound placement, lock that word, mark its chip, clear the anchor, and play the tone. Either endpoint may be the anchor. If it is not, do not buzz. That cell becomes the anchor. Found words stay.
- A bank chip: `reveal`. Found is unchanged.

The highlight under the pointer before the dwell finishes is not a choice. Preview is not a choice. While an anchor is set, cells on the straight line from the anchor to the current cell light when the letters from the anchor are a prefix of one unfound word or of that word reversed. Preview may light as soon as the look is on that cell. It does not wait 200 ms. It never commits. Looking off the line clears the preview and keeps the anchor.

A found word's cells stay gold. They stay buttons. Marking that word again does not append it. The segment is not an unfound answer, so the new cell becomes the anchor.

### Idle pulse

After 20000 ms of `step` time with no finished dwell, set `pulse` to the first letter of the first unfound word in bank order. The first letter is the placement's start cell (`row`, `col`), not the far end. Do not pulse at the start of the puzzle. The pulse lasts 650 ms of `step` and then `pulse` returns to null. It does not set the anchor and it does not lock the word. It happens once per puzzle. A later idle does not pulse a second word. Reduced motion does not animate it. This game has no sitting countdown, so the 120 second clamp does not apply. One `step` of 20000 ms is legal.

### Size

**Revised 2026-10-19.** A look is a point on the glass. The page does not scroll. `html` and `body` use `height: 100dvh` and `overflow: hidden`. There is no 45 vh bottom padding and no 150 px top margin. The banner sits in the chrome row, between Fullscreen and Back, and it is not a hit target.

Chrome side starts at 72 px. It shrinks when 14 percent of the height, or `(w − 24) / 5`, is smaller, and it does not go under 36 px. Play content starts at `8 + chrome + 8`. Mute and Full are that square, 8 px from the corner. Back is at most 176 px wide. Hit boxes match the painted buttons. Do not inflate them.

`fit` is true only when every target is inside the passed viewport and each side is at least 8 px. That includes 300 by 600. The old rule that a width under 320 returns `fit: false` and refuses to shrink is retired.

The puzzle shows every row and every column. `col0` stays 0. Letter buttons are rebuilt only when the puzzle or the chosen size changes, not on every frame.

Cell side is the largest integer that leaves room for the chrome, the full grid, the bank, and a revealed picture. Try a gap of 8, then 4, then 2. Do not grow a cell past 168 px. A text bank chip is at most 72 px tall. A young picture chip is square and at most 112 px. The bank sits under the grid. Group chips are square, at most 160 px, using the column count that makes the chip largest and then uses the fewest rows. Menu buttons are at most 72 px tall and shrink, with the title, until the card ends inside the viewport.

Checked viewports, all with `fit` true and no target outside: 390 by 844, 1024 by 768, 300 by 600, 1280 by 800, 2000 by 1125, and 780 by 493. Shopkeeper kitchen is 36 cells in 6 columns. Tiny Tots body is 15 cells in 5 columns. Little Pals snack is 20 cells in 4 columns. Rush clothing is 42 cells in 6 columns. At 2000 by 1125 a Shopkeeper cell is between 80 and 180 px. Group chips there are at most 160 px. In the live window, `scrollHeight` stays within `innerHeight`, and every visible button stays inside the window, including after a young bank picture opens.

### Marks

**Revised 2026-10-19.** This replaces the line that a found word's cells stay gold, and the color note "found cell `#f3d48a`" as the letter fill. A found word is one continuous highlighter band through the centers of its letters. The band is an SVG line on the grid, `pointer-events: none`, stroke `#e2a62e`, opacity 0.78, round caps, width `max(6, 0.72 × cell)`. It is redrawn in place and does not rebuild the letter buttons. A click on a banded letter still hits that letter. Letter buttons have no border and a transparent background. The bank chip for a found word may still fill gold. The preview wash and the inset green anchor ring stay.

## Generator

The functions are pure. The same arguments return the same value. They do not read the clock, the DOM, the query string, the network, or `speechSynthesis`. They live in this page's script. There is no shared file. `engineSelfCheck` in this file does not generate a sudoku, a math problem, a flash-card trial, or a maze.

### RNG

`makeRng(seed)` takes a uint32. `rngUint(rng)` returns the next uint32. `>>>` is the unsigned shift. `imul` is `Math.imul`.

1. `state = seed >>> 0`.
2. `state = (state + 0x6D2B79F5) >>> 0`.
3. `t = imul(state xor (state >>> 15), state or 1)`.
4. `t = t xor (t + imul(t xor (t >>> 7), t or 61))`. The `t xor …` is an unsigned 32-bit value.
5. Return `(t xor (t >>> 14)) >>> 0`.

`rngInt(rng, lo, hi)` is `lo + (rngUint(rng) % (hi − lo + 1))`, both ends included. `rngShuffle(rng, items)` copies the array and runs Fisher-Yates from the last index down to 1: swap `i` with `rngInt(rng, 0, i)`.

Seed 1 must yield these first four uints, in order: `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`.

`playSeed(clockMs, counter)` is `imul( (clockMs >>> 0) xor imul(counter >>> 0, 0x9E3779B9), 0x85EBCA6B ) >>> 0`. `playSeed(1, 1)` is `0x3B230FE8`. The pure generator never calls `playSeed` and never calls `Date.now`. The page calls `playSeed(Date.now(), counter)` only when `seed(n)` was not called. `counter` starts at 1 and increases by 1 for each unpinned generate. The self-check passes a fixed seed and does not read the clock.

### Directions

Row increases downward.

| Name | Delta row, column |
| --- | --- |
| `right` | 0, +1 |
| `left` | 0, −1 |
| `down` | +1, 0 |
| `up` | −1, 0 |
| `downRight` | +1, +1 |
| `downLeft` | +1, −1 |
| `upRight` | −1, +1 |
| `upLeft` | −1, −1 |

The play set is the band list. Backward on Rush appends `left`, `up`, `upLeft`, `upRight`, in that order, after the four forward names. The span scan is always all eight names, in the table order above, whether or not Backward is on.

### Denylist

After filler, each of these has zero spans. There is no other dictionary. They are a filter. They are never answers, never bank words, and never spoken.

`ASS`, `FAG`, `FUK`, `NIG`, `SEX`, `TIT`, `CUM`.

### Hygiene

`wordHygiene(words)` returns `{ ok, reason }`. `reason` is `""` when `ok` is true, otherwise one of `empty`, `alphabet`, `duplicate`, `substring`.

Reject, and do not retry, when any of these fail.

1. The list is non-empty. Every word matches `A`–`Z` with no space and no lowercase.
2. No duplicates.
3. For two different words `A` and `B`, `A` is not a contiguous substring of `B` and not a contiguous substring of the reverse of `B`. `RAT` with `TAR` fails. `CAT` inside `CATCH` fails. One palindrome, such as `EYE` or `MOM`, is allowed, because the reverse is the same word and the rule is about two different words. `CAT` with `ATE` is allowed. `BUN` with `NUT` is allowed.

Every group in this spec returns `ok`. An illegal list is an authoring error. `wordSearchGenerate` returns `{ ok: false }` immediately and does not place and does not load a fallback.

### Spans

`wordSpans(grid, word)` is how many distinct cell-sets spell `word`. A span is the set of cells, not the direction. Scan every start cell and all eight deltas. When the letters match, record the set. Canonical key: cell indexes `r * cols + c`, sorted ascending, joined with commas. Forward and backward reads of a palindrome share a key and count once.

On a 1 by 3 grid `MOM`, `wordSpans` of `MOM` is 1. On a two-row grid `CAT` / `CAT`, `wordSpans` of `CAT` is 2. On one row `CATE`, `CAT` and `ATE` each have one span and share a cell.

### Generate

`wordSearchGenerate(group, seed, harder)` returns `{ ok, grid, placements, fallbackUsed, directions }`.

`group` is `{ id, band, rows, cols, words, directions, fallback, requireShare }`. `words` are uppercase. `fallback` is an array of row strings, one character per column. `harder` is false unless the page's `harder` boolean is set, and it changes the play set only when `band` is `rush`.

`requireShare` is false only for the six Little Pals play groups `snack`, `toys`, `animals`, `clothes`, `home`, and `outside`. It is true for every other play group and for every fixture below.

1. Run hygiene. On failure return `{ ok: false }` immediately.
2. Start at the group's `rows` and `cols`. Build the play set. If `harder` and the band is `rush`, append the four backward names.
3. From `makeRng(seed)`, assign each word a key from `rngUint`. Sort by length descending, then by that key ascending. A smaller key comes first when lengths tie.
4. Search. A placement attempt is one candidate the search actually tries. The cap is 4000 attempts at this size. Counting happens before the recursive call. Attempt 4001 is not placed.
   - List every in-bounds start and every play direction. Enumeration order, before sorting, is row 0 to the last row, and inside a row column 0 to the last column, and inside a cell the play directions in play-set order. A candidate is in bounds when every letter of the word lands on the grid. It is legal when every cell it touches is empty or already equals the letter it needs. Score is the number of those already-filled cells. Empty is `""`, not a letter.
   - Sort by score descending. The sort is stable. Then walk tie groups of equal score and `rngShuffle` each group. Do not reorder across different scores.
   - Try candidates in that order. Place the word, recurse, and restore the cells this word wrote on failure. Cells that were already filled stay filled with the letter they had.
   - At a full placement, reject it (restore and keep searching) unless every answer has `wordSpans === 1` and, when `requireShare` is true, at least two placements share a cell. Do not reject for occupancy. Do not reject for an overlap ratio under 1.15. When `requireShare` is false, a full placement with one span per word is legal even when no cell is shared.
5. Filler runs only on a placement that passed step 4. Collect empty cells in row-major order. Up to 30 times, shuffle those cells and, for each, shuffle `A` through `Z` (that alphabet order, then `rngShuffle`). Keep the first letter that leaves every answer at exactly one span and every denylist word at zero spans. The scan is the whole grid after that letter. If a cell has no such letter, restore the grid to the pre-filler placement and that restart fails. If all 30 fail, the placement is rejected and the search backtracks, still inside the 4000 cap. A filler restart is not a placement attempt.
6. If the cap hits with no filled grid: on Rush, if the grid is still 6 by 6, add one row (7 by 6) and repeat steps 3–5 once with a fresh `makeRng(seed)` of the same seed and a fresh 4000 cap. The failed 6 by 6 rng is discarded. Tiny Tots, Little Pals, and Shopkeeper do not grow. A 7-letter word has no in-bounds candidate on 6 by 6, so that search fails and then grows. Do not skip the 6 by 6 search. Do not grow a second time. Do not add a column.
7. A second failure, or a first failure on a band that does not grow, loads `fallback`. Accept it only when hygiene already passed and the fallback grid itself has one span per answer, zero denylist spans, and a shared cell when `requireShare` is true. Recover `placements` by scanning play directions. If a word's one eight-direction span is not a run in a play direction, reject the fallback. If a palindrome matches a direction and its opposite, store the direction that appears first in the play set. Otherwise return `{ ok: false }`. The page must not show that.

`placements` is `{ word, row, col, dir }` for the reading direction. The two endpoints are `(row, col)` and the cell `length − 1` steps along `dir`. The page may commit either order. The engine does not score a bank id. `directions` on the return value is the play set, not the per-word dirs. `grid` is an array of row strings. `fallbackUsed` is false when the search filled a grid, and true when the stored rows were returned.

This enumeration is locked. Seed 1 on each fixture below returns `fallbackUsed` false and the rows written in that fixture. Seed 1 on each of the 26 play groups returns `fallbackUsed` false and the rows written under that group. Clothing and Wild animals also report 7 rows and 6 columns. Every other Rush group at seed 1 reports 6 and 6.

### Fixtures

These seven objects are check data. They are not menu groups. `group()` cannot open them. `engineSelfCheck` passes the object to `wordSearchGenerate`. `requireShare` is true on all seven.

| Id | Band | Words | Rows, also the seed-1 grid |
| --- | --- | --- | --- |
| `pets` | tiny | `COW`, `OWL`, `HEN` | `COWLE`, `STRMX`, `HENDH` |
| `foods` | tiny | `BUN`, `NUT`, `PIE` | `BUNUT`, `NOQLI`, `YPIEW` |
| `body` | tiny | `EAR`, `ARM`, `EYE` | `EARME`, `STRMX`, `EYEDH` |
| `toys` | tiny | `BAT`, `TOP`, `CAR` | `BATOP`, `NOQLI`, `YCARW` |
| `little-farm` | little | `EAR`, `ARM`, `CAKE`, `FROG`, `DUCK` | `LEYX`, `FROG`, `EARM`, `DUCK`, `CAKE` |
| `shop-animals` | shop | `CAKE`, `CARD`, `FISH`, `FROG`, `LION`, `DUCK` | `XPFROG`, `EXCARD`, `GNYDLU`, `FISHIC`, `CAKEOK`, `OBOHNI` |
| `rush-animals` | rush | `LION`, `FROG`, `BEAR`, `WOLF`, `CRAB`, `DEER`, `GOAT`, `DUCK` | `CRABDV`, `WCTELD`, `NFEAIU`, `XRLROC`, `WOLFNK`, `XGOATQ` |

`pets` uses the Farm words. The menu id remains `farm`. `toys` here is not Little Pals Toys. `shop-animals` is not Shopkeeper Animals. `rush-animals` is not Wild animals. `little-farm` is not a play group. Its check still requires a shared cell. `EYE` still has span 1. `BUN`/`NUT`, `EAR`/`ARM`, `COW`/`OWL`, and `BAT`/`TOP` are the on-row chains in these fixtures.

Seed 1, `harder` true, on fixture `rush-animals` returns `fallbackUsed` false, does not grow, and returns this grid:

```
KCUDHD
CGYEGG
WRBEOL
BEARAI
PNFBTO
EWOLFN
```

Placements on that grid: `DUCK` `left` at row 0 column 3, `DEER` `down` at row 0 column 3, `CRAB` `downRight` at row 1 column 0, `GOAT` `down` at row 1 column 4, `LION` `down` at row 2 column 5, `BEAR` `right` at row 3 column 0, `FROG` `upRight` at row 4 column 2, `WOLF` `right` at row 5 column 1. The play set is the eight names. At least one placement uses `left`, `up`, `upLeft`, or `upRight`. Every answer still has one span. `DUCK` and `DEER` share the cell at row 0 column 3.

### engineSelfCheck

`engineSelfCheck()` returns `{ ok, failures }`. Each failure is one sentence naming the case. It does not call `Date.now`, `setTimeout`, `setInterval`, `requestAnimationFrame`, `fetch`, `WebSocket`, or `speechSynthesis`. It does not read the DOM. Loops stop at the caps, so a failed search still returns.

Order:

1. `rngUint` four times on `makeRng(1)`. Compare to `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`. Compare `playSeed(1, 1)` to `0x3B230FE8`. `rngInt` on a fresh `makeRng(1)` stays inside `0..3`.
2. `wordHygiene(["CAT", "CATCH"])` is substring. `["RAT", "TAR"]` is substring. `["CAT", "CAT"]` is duplicate. `["CA T"]` and `["cat"]` are alphabet. `[]` is empty. `["MOM", "CAT"]` and `["CAT", "ATE"]` are ok.
3. `wordSpans` on `MOM`, on `CAT`/`CAT`, and on `CATE`, as above. `CAT` and `ATE` on `CATE` share a cell.
4. `wordSearchGenerate` seed 1, `harder` false, for the seven fixtures. Assert `ok`, `fallbackUsed` false, the exact rows in the table, every word length matches the band, every placement direction is in the play set, `wordSpans` is 1 for each answer, every denylist word has 0 spans, every letter is `A`–`Z`, and at least two placements share a cell. Shop placements do not use a diagonal name. Rush placements do not use a backward name.
5. Each stored fixture fallback passes those span, denylist, and share checks without running the search. `EYE` has span 1.
6. Seed 1, `harder` true, on `rush-animals`. Assert the exact harder grid and placements above, span 1, denylist 0, and a shared cell.

The page `selfCheck` runs the family cases, then calls `engineSelfCheck` and appends its failures, then runs the page cases, then `family-no-storage`. It writes the JSON and the title, then stops.

## Self-check cases

Family cases use these names. `family-guards` runs first. `family-no-storage` runs last, after the page cases. The others run before `engineSelfCheck`.

1. `family-guards`. At the start, `checking` is true, reconnect is suppressed, and the socket is null.
2. `family-no-storage`. Spy on `setItem` and `removeItem` from the start of the check. After every other case, neither was called. If storage is readable, `pop-pals-v1` is unchanged and there is no key `word-search`.
3. `family-play-dwell`. `seed(1)`, `start("tiny")`, `group("body")`, `releaseLook`. A look on cell `0-0` stepped 640 ms does not set `anchor`. A look stepped 660 ms sets `anchor` to `0-0`. Then `seed(1)`, `group("body")` again, and `tap` on cell `0-0` sets `anchor` to `0-0` with no dwell. That is the same writer.
4. `family-menu-dwell`. `go("menu")` first. A look on Let's go for 600 ms leaves `screen` at `menu`. A look for 1300 ms sets `screen` to `group` and `level` to `tiny`.
5. `family-pointer-blocks`. `seed(1)`, `start("tiny")`, `group("body")`. On cell `0-0`, `hold`, then a 660 ms look, does not set `anchor`. `release`, then a fresh 660 ms look, does.
6. `family-stale`. A look that `step` ages to 500 ms leaves `dwell()` at 0 and does not commit. Age under 500 is still fresh.
7. `family-target-size`. On the menu, `targetsAt` at 390 by 844, 1024 by 768, 300 by 600, 1280 by 800, 2000 by 1125, and 780 by 493 has `fit` true. Every target lies inside that viewport. A short side may be under 72 when that is what fits.
8. `family-eyes`. `eyes()` has only the keys `on` and `live`. A fresh test look sets `live` true with the socket still closed. The Eyes chip is shown for that look.
9. `family-mute`. The mute button's visible text is empty. It contains two inline speaker SVGs. The words Mute and Sound are not visible. `tap` sets `state().muted`. A second `tap` clears it.
10. `family-query-inert`. The junk flag's value is not in the document. The screen is still the menu. No socket was opened.
11. `family-plain-state`. `state()` can be turned into JSON and contains `level`, `screen`, `banner`, and `muted`. It does not contain a coordinate, the socket URL, or the query.

Page cases, after `engineSelfCheck`:

1. `hygiene-shipped`. `wordHygiene` returns ok for all 26 play lists and all 7 fixtures. None of the seven denylist tokens is a member of any of those lists.
2. `seed1-groups`. For every play group, `seed(1)` then `group(id)` on that group's band returns `fallbackUsed` false, the rows printed under that group, one span per word, zero denylist spans, and placement directions inside the band's play set. `shared` is true except for `snack`, `toys`, `animals` on little, `clothes`, `home` on little, and `outside`, where `shared` is false. `body` and `farm` are 3 by 5. Every little group is 5 by 4. Every shop group is 6 by 6. `clothing` and `wild-animals` are 7 by 6. Every other rush group is 6 by 6.
3. `one-span`. On the seed-1 `body` grid, `EYE` has span 1. On the seed-1 fixture `little-farm` fallback, every word has span 1.
4. `chain-tiny`. The seed-1 `body` grid has `EAR` and `ARM` sharing a cell, and every placement direction is `right`.
5. `anchor-clear`. `releaseLook` between looks. A 640 ms look does not set `anchor`. A 660 ms look does. After `releaseLook`, a second 660 ms look on that same cell clears it. `found` stays empty.
6. `commit-either-order`. On a fresh seed-1 `body` puzzle, anchoring the first cell of `EAR` and committing the last adds `EAR` to `found` once. On a second fresh puzzle, anchoring the last and committing the first does the same. A further commit of those ends does not append `EAR` again.
7. `not-a-word`. A segment that is not an unfound answer leaves `found` unchanged, sets `anchor` to the new cell, and does not create a miss field. The banner does not contain fail, wrong, lose, or miss. `said` is unchanged.
8. `preview-no-commit`. With an anchor on the first letter of an unfound word, `look` on the next letter of that word and `step` 200 ms does not add to `found`. There is no 200 ms gate: the preview is allowed at once, and it still does not commit.
9. `leave-grace`. On a cell, accumulate 400 ms, leave for 200 ms, return, and accumulate 250 ms more. The anchor sets, because on-button time reached 650 and the gap was under 280. On a fresh cell, accumulate 400 ms, leave for 280 ms, return, and accumulate 250 ms. The anchor does not set. A further 660 ms on that return visit does set it. The anchor from the first half of this case is not cleared by the leave.
10. `bank-speaks`. On Tiny Tots `body`, a 600 ms look on the `EAR` chip leaves `revealed` null and `said` null. A 1300 ms look sets `revealed` to `EAR`, `said` to `EAR`, and `picture` true, and leaves `found` unchanged. A 640 ms look that leaves the chip does not set `said`. If `speechSynthesis` exists, the only utterance text is `EAR`.
11. `cell-silent`. A 660 ms look on a cell does not change `said`. A bad segment does not change `said`.
12. `shop-no-diagonal`. On seed-1 `kitchen`, every placement direction is `right` or `down`. `state().directions` is exactly `right`, then `down`. Anchor cell `0-0` and commit cell `1-1`. That pair is not an answer's endpoints. `found` stays empty and `anchor` becomes `1-1`.
13. `harder-off`. `start("rush")` with `harder` false. `group("orchard")` after `seed(1)`. `directions` is `right`, `down`, `downRight`, `downLeft`. No placement uses `left`, `up`, `upLeft`, or `upRight`. Anchoring the last cell of a placed word and committing the first still adds that word once. That is either-order, not Backward.
14. `harder-on`. `backward()` on the Rush group screen sets `harder` true. `backward()` after `start("tiny")` does not. With `harder` true, seed 1 of fixture `rush-animals` matches the harder grid in the generator section, and `directions` is the eight names in play-set order. `wordSpans` is 1 per word. The `harder` flag's value string is not in the page.
15. `fallback-unique`. `useFallback("body")`, `useFallback("snack")`, `useFallback("kitchen")`, and `useFallback("orchard")`, each after `start` of that band, shows the stored rows. Body, kitchen, and orchard pass the share check. Snack does not share a cell, has one span per word, and is 5 by 4. `fallbackUsed` is true.
16. `no-drag`. `hold` on one end of `HEN` and `release` on the other leaves `found` unchanged and does not set the anchor. Two taps, one on each end, add `HEN` once.
17. `cell-size`. After Shopkeeper kitchen, every checked viewport reports 36 cells, 6 distinct cell columns, no Previous or Next, and `fit` true. A cell is at most 168 px. At 2000 by 1125 the cell side is between 80 and 180. Tiny Tots body at 390 by 844 reports 15 cells and 5 columns. Little Pals snack at 390 by 844 reports 20 cells and 4 columns. Rush clothing reports 7 rows, 42 cells, and 6 columns, and it fits at 2000 by 1090 and at 390 by 844.
18. `fit-no-scroll`. Shopkeeper and Rush group screens fit the same viewports. Rush still has Backward. Shopkeeper group chips at 2000 by 1125 are at most 160 px. The live window does not grow `scrollHeight` past `innerHeight`, and no visible button leaves the window, on the menu, on a Shopkeeper puzzle, on the Tiny Tots groups, and after the Body picture opens. `col0` stays 0. The puzzle draws every letter and does not draw a pager.
19. `group-reject`. `group("not-a-group")` does not change the screen and does not insert that id into the page. On Tiny Tots, `group("kitchen")` does nothing. On Shopkeeper, `group("animals")` places `FROG`, `DUCK`, `BEAR`, `WOLF`, `LION`, `GOAT`, `MULE`, not the fixture list.
20. `cap-rest`. Locking every word calls `go("rest")`. `puzzlesDone` is 1. The banner is "All the words." Another is a live target. It does not contain a grade. A 600 ms look on Another stays on rest. A 1300 ms look sets `screen` to `group` and does not change the band. Finish a second puzzle. `puzzlesDone` is 2. Another is not a live target. `state()` has no miss, score, or clock field.
21. `pulse-once`. `releaseLook` so no dwell is running. From the puzzle's start time, `step` of 19999 ms leaves `pulse` null and `anchor` null. `step` at 20000 ms sets `pulse` to the start cell of the first bank word. `found` is empty. `step` 650 ms later clears `pulse`. Another 20000 ms does not set `pulse` again.
22. `seed-pin`. `seed(4)` during a puzzle does not change `grid`. The next `group` of that id uses seed 4. A second `group` without `seed` does not read the old pin. `seed(1)` then `group("body")` returns the Body rows.
23. `no-remote`. The document has no `src` or `href` to a network URL. No image element loads a file. Speech, if called, was only an authored word.

`selfCheck` sets `ok` false if any failure list is non-empty. It does not read screenshots and it does not wait on a frame.

## README paragraph

Add this link to the Play list, in the same list, after Saddle Up!:

- [Word Search](https://calledtoconstruct.github.io/games/word-search/)

Add this section after the Saddle Up! section and before Add a game.

## Word Search

A word puzzle for ages 2 and up. Look at the first letter, then look at the last letter. The word between them locks when those two looks match. You do not drag a line through the letters. Tiny Tots get three short words, all across, and a picture for each word. Little Pals get a few more words, still across, still with pictures. Shopkeeper adds down. Rush adds the slanted ways, and a Backward button for the other direction. A long look at a word in the bank says that word. On the younger bands it also shows the picture. That look does not by itself find the word.

Open `word-search/index.html` in a browser, or play [Word Search](https://calledtoconstruct.github.io/games/word-search/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at a letter until the ring fills, then look at the other end until the ring fills. The letter cells are the play targets, and every letter is on the screen. The bank, the group names, Backward, Back to Menu, Mute, and Fullscreen take twice as long, so reading one does not press it. Mouse and touch work in the same puzzle. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

The README does not mention the check flag, the window hook, or a score.

## Key Decisions

- The clock is the architecture clock. Play is 650 ms. Menu is 1300 ms. The ring fills across the whole dwell, `elapsed / D`. There is no 200 ms onset and no fill formula that starts at 200 ms. `LEAVE_MS` is 280, the grace before an in-progress dwell is dropped. A finished anchor survives the grace. 640 ms does not commit. 660 ms does.
- Preview may show during the whole dwell. It still does not commit. Leave is not instant. It waits 280 ms. There is no silent first 200 ms.
- Grid size is the engine size. Tiny Tots is 3 rows by 5 columns, not 4 by 3 and not the age lock's 3 by 4. Little Pals is 5 rows by 4 columns, not the age lock's 4 by 5. Rush grows once, by one row, to 7 by 6, never by a column. The attempt cap is 4000 per size, not 20000.
- The span scan is always all eight directions. Backward only adds placement directions on Rush. Either-order marking of a word that was placed forward does not need Backward. The two dwells are the two ends. A drag never marks a word.
- Overlap means at least one shared cell. A one-span grid that already shares a cell is not rejected for a ratio under 1.15 and is not rejected for being too empty or too full.
- Little Pals is the exception. On a 4-column row, two right-only words share a cell only when two different 3-letter words sit in columns 0–2 and 1–3 and the letters agree. A 4-letter word fills the row, and a 3-letter word on that row would be a substring, which hygiene already forbids. None of `BUN`/`NUT`/`PIE`, `NET`/`TOP`, `PUP`/`PIG`, `HAT`/`TIE`, `TUB`/`BED`/`RUG`, or `BUD`/`DEW`/`LOG`/`SUN`/`HAY` agree. The age chains such as `BUN`+`NUT` are five letters and do not fit. Those six lists keep `requireShare` false. Every other group and every fixture keeps `requireShare` true. Do not add a column, do not turn on `down`, and do not edit the words to force a chain.
- Word lists and ids are the 26 groups in this spec, taken from the age names. The seven fixtures stay in the file for `engineSelfCheck` and are not offered on the group screen. Body's fallback is the `body` fixture, because the age grid was 3 by 4. Farm's fallback is the `pets` fixture rows, because those are the same three words and the age grid was 3 by 4. Every other age fallback was the wrong size and had no fixture with those words, so the seed-1 rows in this spec are the fallback.
- Speech is allowed in this game, and only here as specified: one authored word, spoken when a bank dwell of 1300 ms finishes, or when that chip is tapped. The architecture line that this game does not speak is not the rule. Cells, misses, and menus stay silent. `said` is a state field. Shopkeeper and Rush speak the word and do not show a picture.
- A bank dwell is a menu dwell because it does not mark the word. A play dwell stays 650 ms and does not speak.
- A miss moves the anchor. Found words stay. There is no tally and no scolding banner.
- The sitting ceiling is 2 puzzles on every band. Another returns to the group screen once. It does not call `start` and it does not pick a group.
- `?selfcheck` is synchronous. It does not open a socket, does not wait, and does not apply deep links.
- This file's `engineSelfCheck` covers the RNG and word search only. The other four games keep their own pages.
- Seed 1 grids are part of the contract, including the fixture rows and the harder `rush-animals` grid. A different tie-break order is a failed check, not a private choice.

## Out of scope

- A drag, a stroke, a blink, or a wink as the commit.
- A 200 ms onset, a ring that stays empty until 200 ms, or a contracting mark on the cell.
- A third dwell time, a longer child dwell, or a hidden quiet period inside 650 ms.
- `localStorage`, cookies, `fetch`, remote art, remote fonts, or a second script.
- Speech of anything except one bank word from that group's list. No speech on a cell, on a miss, on a menu, or when the puzzle appears.
- A score, a streak, a rank, a life, a buzzer, a mistake count, or a banner that says fail or wrong.
- A clock that can end the puzzle. A Done button. The early way out is Back to Menu.
- An English dictionary beyond the seven denylist tokens. Showing those tokens as answers or speaking them.
- Rejecting a legal grid for an overlap ratio under 1.15, or chasing a board that is 80 percent full.
- A column past 6. A second grow. An attempt cap of 20000. The age grids of 3 by 4 and 4 by 5.
- Backward on Tiny Tots, Little Pals, or Shopkeeper. A fifth band.
- Replacing a Little Pals list with `little-farm`, or replacing Shopkeeper Animals or Wild animals with the fixture lists.
- Putting sudoku, math, flash cards, or the maze into this file.
- A typed answer, a hint that marks a word found, or a keypad.
- Editing Pop Pals, Sugar Counter, Sun Garden, or Saddle Up!.
- A shared gaze file or a bundler.

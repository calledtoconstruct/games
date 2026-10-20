# Sudoku

**Date.** 2026-10-18.

**File.** `/home/joseph/code/games/sudoku/index.html` only. One HTML document. Pictures are canvas drawings. Digits are text on buttons. No second script, no module, no asset folder, no downloaded puzzle.

**Window.** `window.sudoku`, assigned as soon as the script runs, before the query string is handled.

**Title.** Sudoku.

This file is the build spec. It copies the page contract and the Sudoku section of the architecture memo, the Sudoku targets of the gaze memo, the Sudoku content of the age memo, and the Sudoku engine, shared RNG, and `engineSelfCheck` order of the engines memo. Where those disagree, the Key Decisions section is the one to follow.

## Player loop

The child looks at an empty square until the look commits. That look only latches the square. It does not write a symbol. The child then looks at one large symbol. That second look writes it into the latched square and clears the latch. A single look never both picks the square and writes.

Real Sudoku rules hold on every band. Each symbol appears once in its row, once in its column, and once in its box. The board has one solution. A singles-only solver finishes it. There is no pencil mark, no clock, no countdown, no mistake tally, no life, no score, no streak, and no rank.

A miss does nothing. Looking at a given, at the background, or at a symbol while nothing is latched does not write, does not buzz, does not say fail, wrong, or no, does not reset the board, and does not clear symbols already placed. Every symbol stays a button and looks the same. A symbol that would repeat in the row, the column, or the box is not written.

Undo removes the last symbol the child placed. It does not remove a given and it does not drag. Hint latches one cell that is a single right now. It does not mark, dim, or pulse a tray symbol, and it does not write. The child still chooses the symbol.

**One screen revised 2026-10-20.** Number Grid and Big Board stay on one screen. The whole grid and every digit are showing together. Looking at an empty square latches it and does not open another screen. The sentences below that describe a box chooser, a zoomed box, or a separate symbol screen are superseded for play. A finished board is the same full grid, read-only. The banner is a pill in the open part of the top row, so "Look at an empty square.", "Now look at the symbol.", and "That board is done." stay readable beside New board and Back to Menu. On a narrow window that pill moves to its own row under those buttons, and the grid starts below it.

**Play revised 2026-10-19.** The tray shows every symbol for that board, on every band, and none of them is marked as the answer. Picture Squares still starts with 12 of 16 filled, so each empty square has one picture that fits, but the tray shows all four pictures. A 6×6 box chooser is two boxes across and three bands down, the same shape as the grid. A 9×9 chooser stays three by three. When the tray does not fit under a zoomed box, latching an empty square opens the symbol screen. A deal is shown only when the finished grid and the givens each use a symbol once per row, column, and box, the givens match that finished grid, and the puzzle has one solution. The sentences below that say a dim tray, a one-symbol Picture Squares tray, a 3×2 of six boxes, or a hint symbol are superseded by this note. The README block at the end of this file is the 2026-10-18 text. The live README was updated with this revision.

When the grid matches the solution, the board is finished. The page goes to the rest card. The banner is one calm sentence. It is not a number, a time, or a rank. Another, if the sitting still has a board left, deals the next board. After the second finished board, Another is gone. Back to Menu ends the sitting early. Caps are ceilings, not quotas.

**Finished board revised 2026-10-20.** The finished grid stays on the page, read-only, with no tray. Undo, hint, and a placed symbol do not change it. The banner still says "That board is done." Another sits under the grid when another board remains. The rest card does not cover the grid.

Mouse, touch, and gaze share the same writers. A click or a tap is that writer with no wait. A gaze commit is the same writer after the dwell below. Nothing in the game drags.

### What the child hears

Sound is an oscillator started after the first pointer press or the first committed gaze action. It is not a recorded file. Master gain is 0.5 when sound is on and 0 when muted. Mute does not remove buttons.

- A successful write plays a sine at 523 Hz for 70 ms.
- The write that finishes the board plays a sine at 659 Hz for 90 ms instead of the 523 Hz tone.
- A miss, a dead symbol, a given, Undo, Hint, and menu buttons play nothing.

There is no `speechSynthesis` anywhere in the file.

`prefers-reduced-motion: reduce` skips the hint pulse and any other decorative motion. The latch, the write, and the rest card still happen.

### Banner sentences

`#banner` is one status line. It does not receive clicks. Its text is assigned with `textContent`. It never contains fail, wrong, mistake, score, rank, or a clock.

| When | Text | Shown |
| --- | --- | --- |
| Menu | empty | hidden |
| Board, box chooser, nothing latched | Look at a box. | shown |
| Board, flat grid or zoomed box, nothing latched | Look at an empty square. | shown |
| A cell is latched, or the symbol screen is up | Now look at the symbol. | shown |
| Rest | That board is done. | shown |

Hint does not change this table, except that hint latches a cell, so the latched sentence applies. A miss leaves the banner as it was.

### Pictures

Tiny Tots and Little Pals use four pictures, shuffled onto the values 1, 2, 3, and 4 for that board. Shopkeeper and Rush use large numerals, not these pictures. Each picture is one object, flat fills, high contrast, no letters, no face, no logo, and no downloaded file.

| Value name | Drawing |
| --- | --- |
| Sun | A yellow disk, `#f5c518`, centered, radius about 28% of the button's short side. Eight short orange rays, `#f08a24`, evenly spaced, each about 18% of the short side long. No face. |
| Moon | A pale yellow crescent, `#f3e6a2`, opening to the right. Paint a disk, then remove a second disk shifted right by about 35% of the radius. No face. |
| Star | A five-point gold star, `#e2b007`, point up, outer radius about 42% of the short side. No face. |
| Heart | A red heart, `#d62828`, two round lobes and a point at the bottom, centered. No face. |

The shuffle is `rngShuffle(makeRng(seed), ["sun", "moon", "star", "heart"])` on a new generator seeded with the same seed as the puzzle. It does not share the puzzle generator's state. Value 1 draws entry 0, value 2 draws entry 1, and so on. Picture buttons use those names as their accessible names. Digit buttons show the digit in the page font at about 60% of the button's short side, ink `#2c2416`, centered in the square and in the tray.

Given cells and child-placed cells draw the same symbol. A child-placed cell uses a sun-gold ground `#fff6df` so it can be told from a given. A given uses the card ground `#fffdf6`. Neither cell is a stack of candidates.

## Bands

Band ids, matching the menu buttons, are `tiny`, `little`, `shop`, and `rush`. Do not rename them. Do not drop Rush. Do not add a fifth band.

| Band | Ages | Grid | Boxes | Symbols | Givens | Tray while a cell is latched | Sitting |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Tiny Tots | about 2–3 | 4×4 | four 2×2 | four pictures | exactly 12 of 16 | only the one symbol that fits that cell | 2 boards at most: the first, then one optional Another, then stop |
| Little Pals | about 4–5 | 4×4 | four 2×2 | four pictures | 8, 9, or 10 of 16 | all 4 pictures; illegal ones dim and not controls | same ceiling, 2 |
| Shopkeeper | about 6–8 | 6×6 | six 2×3 | digits 1–6 | 24 through 28 of 36, and every row has at least 4 givens | digits 1–6; illegal ones dim and not controls | 2 |
| Rush | about 9 and up | 6×6, or 9×9 when a full-grid cell would be at least 72 px | 6×6: six 2×3. 9×9: nine 3×3 | 6×6: digits 1–6. 9×9: digits 1–9 | 6×6: 18 through 22 of 36. 9×9: 40 through 48 of 81 | the digits of that grid; illegal ones dim and not controls | 2 |

Seeds 1 and 2 land on the floor of the window: 12, 8, 24, 18, or 40. A generated board inside the window is legal. Do not aim for a minimal puzzle. There is no 17-clue board.

Distinct given symbols: at least 3 of the 4 on a 4×4, at least 5 of the 6 on a 6×6, and at least 8 of the 9 on a 9×9.

Empties are spread across boxes. The fullest box has at most one more empty cell than the emptiest box.

Technique:

- Tiny Tots. Every empty cell is a naked single on the opening grid, all at once, before any write. `sudokuOpeningNaked` is true, and every empty cell's candidate list has length 1. The child latches an empty cell, then the one picture in the tray. That is the whole move.
- Little Pals. Naked singles only, as a chain. `sudokuSolveSingles(size, grid, "naked")` equals the solution. A hidden single is not required.
- Shopkeeper. Naked singles, then hidden singles. No guess. `sudokuSolveSingles(size, grid, "both")` equals the solution, and `sudokuCountSolutions` is 1.
- Rush. Same as Shopkeeper, on both 6×6 and 9×9.

The page picks Rush's size when it generates, from the live window width. Let a full-grid cell be `floor((width − 16 − 8 × (size − 1)) / size)` with an 8 px page pad and an 8 px gap. If that value for size 9 is under 72, generate a 6×6. Otherwise generate a 9×9. The engine still generates a 9×9 when asked. Shopkeeper is always 6. Tiny Tots and Little Pals are always 4.

Showing a full 6×6 or 9×9 is a separate, stricter test, in Gaze targets. A 9×9 that was generated on a wide window stays a 9×9 if the window later becomes narrow. The page then uses boxes. It does not swap in a 6×6 and it does not paint cells under 72 px.

## Screens and API

### Skeleton

The head holds charset, a viewport meta, `<meta name="theme-color" content="#f4f0d8">`, and the title Sudoku. Nothing else is loaded. No remote image, font, style sheet, script, or beacon. No `localStorage`, cookie, beacon, or `fetch`.

Colors and type, matching the shipped horse game: paper `#f4f0d8`, card `#fffdf6`, ink `#2c2416`, sun `#ffc857`, sun shadow `#e2a62e`. The font stack is Arial Rounded MT Bold, then Trebuchet MS, then Segoe UI. The gaze ring is `#2f8a3e`.

The body, in this order:

1. `#view`, a full-page canvas, `pointer-events: none`, `aria-hidden="true"`. It is not a hit target. Symbol canvases live inside the buttons. `#view` itself draws nothing.
2. Chrome that is always in the tree: `#mute`, `#full`, `#back`, `#eyes`, `#gaze`, and `#banner`.
3. `#screen-menu`.
4. `#play`, the board. Every board control lives here.
5. `#screen-rest`.
6. `#selfcheck-result` is not in the authored markup. The loader creates that `pre` only when `selfcheck` is present.

`go` shows one screen and hides the other two. `#play` is the board screen. `#back` is hidden on the menu and shown on the board and on rest. Unknown screen ids do nothing. Helpers may call `go`. They may not assign the screen themselves.

### Menu

`#screen-menu` holds:

- The heading Sudoku.
- The line `Look at an empty square, then at the symbol that belongs there.`
- The line `Ages 2 and up`.
- Four band buttons, ids `tiny`, `little`, `shop`, and `rush`, labels Tiny Tots, Little Pals, Shopkeeper, and Rush. Each has a short line: `Ages 2 to 3. Four pictures, and a very full board.` / `Ages 4 to 5. Four pictures, and more empty squares.` / `Ages 6 to 8. Digits on a six by six board.` / `Ages 9 and up. Digits, and a nine by nine board when it fits.`
- `#lets-go`, label `Let's go`, sun-gold.

Tiny Tots starts highlighted (`aria-pressed="true"`). A band button only highlights that band. It does not deal a board. Let's go calls `start` for the highlighted band.

### Board

`#play` holds the grid or the box chooser, the tray, `#undo` labeled Undo, `#hint` labeled Hint, `#new-board` labeled New board, and `#boxes` labeled Boxes.

Boxes is hidden on a flat 4×4 and hidden on the box chooser. It is shown on the zoomed box and on the symbol screen. Its label is the word Boxes, never Back to Menu.

New board is shown for the whole board screen and hidden on the menu and on rest.

### Rest

`#screen-rest` holds the sentence `That board is done.` and `#another` labeled Another. It uses the same Back control. It does not show a grade, a miss count, a time, or a rank. Another is in the tree only when the board was solved and `dealt` is 1. After the second board, Another is not in the tree. The 2026-10-20 note above keeps the grid visible; this card does not cover it, and the banner is the sentence.

### Sitting counter

`dealt` counts boards dealt in this sitting.

- `go("menu")` sets `dealt` to 0 and clears the puzzle.
- `start(band)` resets the puzzle, sets `dealt` to 1, generates, and calls `go("board")`. This is a new sitting.
- `again()` is the Another writer. It does nothing when the screen is not rest, when `solved` is not true, or when `dealt` is already 2. Otherwise it generates the next board, sets `dealt` to 2, clears the latch, the undo stack, and the hint, and calls `go("board")`. It does not zero `dealt` first.
- `newBoard()` generates a replacement for the current board, stays on board, clears latch, undo, hint, and placed symbols, and does not change `dealt`.

`start` from the menu, from Let's go, from `play`, and from `rush` is a new sitting. Another does not call that reset, because a reset would wipe the ceiling. The architecture sentence that Another calls `start` means Another deals a new grid through the same generator. The writer that keeps the ceiling is `again`.

### Hook

`window.sudoku` has these functions. `render` runs before any of them return, including while `selfcheck` is on and the frame loop is off.

Shared:

| Function | Behavior |
| --- | --- |
| `start(band)` | If `band` is not `tiny`, `little`, `shop`, or `rush`, do nothing. Do not write that string into the page. Otherwise set the band, highlight it, reset the sitting as above, and go to the board. Do not open the socket. |
| `go(screen)` | The only writer of the screen id. `menu`, `board`, and `rest` only. |
| `look(nx, ny)` | Place a test gaze at window fractions. Do not commit. |
| `releaseLook()` | Drop the test gaze, hide the ring, and set the dwell fill to 0. Do not clear a cell latch. |
| `hold(nx, ny)` | Pointer down. A second hold is ignored while the first pointer is down. |
| `release()` | Pointer up. It does not fire a button. |
| `tap(nx, ny)` | Run the writer for the button at that point, with no dwell. Do not leave the pointer down. |
| `step(now)` | The only clock. |
| `dwell()` | Ring fill from 0 to 1. |
| `eyes()` | `{ on, live }` and nothing else. No coordinates. |
| `centers()` | Window fractions and pixel boxes for every live target in the real viewport. |
| `targetsAt(w, h)` | Those boxes for a pretend viewport. It does not generate a new puzzle and it does not resize the real window. |
| `state()` | Plain data. JSON can carry it. Always includes `level`, `screen`, `banner`, and `muted`. Never includes a DOM node, a gaze coordinate, the socket URL, or the query string. |
| `seed(n)` | Pin the next generate only, as `n >>> 0`. It does not change a board already on screen. It is not read from the query. |
| `selfCheck()` | `{ ok, failures }`. Each failure is one sentence that names the case. Synchronous. |

Sudoku writers:

| Function | Behavior |
| --- | --- |
| `latch(cellId)` | Latch an empty cell. A given, a child-filled cell, or an unknown id does nothing. A second latch replaces the first. A latch does not write. Calling it again on the same cell does not clear it and does not write. |
| `write(symbol)` | Write into the latched cell when `symbol` is an integer in `1..size` and that symbol is live. A string, including `"1"`, `"sun"`, and any markup, does nothing and is not inserted into the page. A dead symbol, an unknown symbol, or a missing latch does nothing. A successful write clears the latch and the hint. If the symbol screen was up, return to the zoomed box. If the grid then matches the solution, set `solved` and call `go("rest")`. |
| `undo()` | Pop the last child placement and set that cell back to 0. Do not change a given. If the stack is empty, do nothing. Clear the hint. Leave the latch if that cell is still empty. |
| `hint()` | If the board is solved, do nothing. Otherwise latch the first single, as defined under Hint order, and set `hint`. Do not change `placed`. |
| `zoom(boxId)` | On a 4×4, do nothing. `zoom(null)` clears the enlarged box and returns to the box chooser. A known id, `box-0` through `box-5` or `box-0` through `box-8`, opens that box. The same id again clears it. Any other string does nothing and is not inserted into the page. |
| `newBoard()` | The New board writer above. Unknown to call off the board: still replaces the puzzle if a band has been started; if no band has been started, do nothing. |
| `again()` | The Another writer above. |

Cell ids are `cell-` plus the row-major index (`cell-0` …). Box ids are `box-` plus the box index. Symbol button ids are `symbol-` plus the value (`symbol-1` …). `state().latched` is the index number, or null, not the DOM id.

### state()

Besides `level`, `screen`, `banner`, and `muted`:

| Field | Value |
| --- | --- |
| `size` | 4, 6, 9, or null when no board is dealt |
| `givens` | Count of original clues, not counting child placements |
| `empties` | Count of cells that are still 0 |
| `latched` | Index number, or null |
| `tray` | `{ symbol, live }` for each offered symbol, in value order |
| `placed` | `{ index, symbol }` in undo order, child writes only |
| `puzzle` | Row-major array, clues plus child writes, 0 for empty |
| `solution` | Row-major solution, symbols `1..size` |
| `solved` | True when every cell equals the solution |
| `hint` | null, or `{ index, symbol }` |
| `singles` | True when the band technique still finishes this grid to `solution` |
| `solutions` | `sudokuCountSolutions` of the current grid: 0, 1, or 2 |
| `zoom` | null, or a box id |
| `view` | null, `flat`, `boxes`, `zoom`, or `symbols` |
| `dealt` | 0, 1, or 2 |
| `fallbackUsed` | The flag from the last generate |

No `mistakes`, `wrongs`, `rank`, `score`, `lives`, `clock`, `msLeft`, or `said`.

On a fresh Tiny Tots board, `tray` has exactly one entry and `live` is true, even before a latch. That symbol is the sole candidate of the lowest-index empty cell. Dwelling it before a latch does not write. After a latch, the one entry is the sole candidate of the latched cell, and it is a real button. On older bands, before a latch, every symbol is listed and none are live. After a latch, a symbol is live only when it is a candidate of that cell.

`singles` uses `sudokuOpeningNaked` for Tiny Tots (true on the fresh board, and still true after a correct write while every remaining empty cell is a naked single). Little Pals uses `sudokuSolveSingles(..., "naked")`. Shopkeeper and Rush use `sudokuSolveSingles(..., "both")`.

### Hint order

Walk cell indexes from 0. The hint cell is the first empty cell whose candidate list has length 1. If the band is Shopkeeper or Rush and no naked single exists, use the first hidden single in the same unit order as `sudokuSolveSingles` mode `"both"`. If none exists, `hint()` does nothing. Tiny Tots and Little Pals do not fall through to a hidden single.

Set `latched` to that index and `hint` to `{ index, symbol }`. If the live view is the box chooser, open the box that contains that cell. If the tray fits on the zoomed box, stay on `zoom` so the symbol can pulse. If the tray would not fit, go to `symbols`. The child still has to dwell the symbol.

The pulse is a soft opacity change on that cell's outline and on the tray symbol. It is not a second ring and it does not commit. Reduced motion keeps a steady outline and does not pulse.

### Query

Flags are presence checks. The value is ignored. The raw search string is never written into text or an attribute. Unknown flags do nothing. No flag changes the socket URL.

- `selfcheck` is handled first, before any frame and before any socket. Set `checking` and `suppressReconnect`. If a socket already existed, close it and keep reconnect suppressed. Call `selfCheck()`. Create a `pre` with id `selfcheck-result`, set its `textContent` to `JSON.stringify` of the result, and set `document.title` to `SELFCHECK_OK` or `SELFCHECK_FAIL`. Then stop. Do not call `requestAnimationFrame`. Do not construct a `WebSocket`. Do not apply `play` or `rush`.
- Otherwise `play` calls `start("tiny")`. If `play` is absent, `rush` calls `start("rush")`. If both are present, `play` wins.

Read the query before `requestAnimationFrame` and before `connectGaze`.

### Paint language

Cream cards, sun-gold primary buttons, ink on cream, the green ring. `#mute` and `#full` are 72 by 72 px. `#full` shows the visible word Full and has accessible name Fullscreen. `#back` says Back to Menu, is at least 72 px on its short side, and is a menu dwell.

`#mute` has no visible text. It contains two inline SVGs, one hidden at a time. The sound-on graphic is a speaker wedge plus one wave. The sound-off graphic is the same wedge plus a cross. The accessible name is `Mute sound` when sound is on and `Turn sound on` when muted. `aria-pressed` follows `muted`. The words Mute and Sound are not in the button's visible text. `tap` on it sets `state().muted`, and a second `tap` clears it. Gain follows that flag. The button stays in the tree.

`#eyes` shows the word Eyes and is not a button. It is shown when the socket is open or a test look is held, and hidden otherwise. `#gaze` is a 54 px circle, `z-index: 60`, `pointer-events: none`, centered on the pointer with `translate(-50%, -50%)`. Its fill is a conic gradient, `#2f8a3e` for the progress and `rgba(244, 240, 216, 0.45)` for the rest. It is shown only while the sample is fresher than 500 ms.

Every live control is a real `button`. Gaze hit-testing is `elementFromPoint`, then the nearest ancestor `button`. The canvas is not a hit target. There is no geometric walk, no blink, no wink, and no drag that commits.

## Gaze targets

Copy the Saddle Up gaze client: the same constant names, the same hello, the same framing, the same dwell choice, and the same pointer guard. The clock numbers in this section win over the gaze memo's onset. Which controls are play or menu, and what a miss does, come from the gaze memo.

### Constants

- `GAZE_URL` is the literal `ws://127.0.0.1:7081`. It is not built from `location`, from a query value, or from a message.
- `PLAY_DWELL_MS` is 650.
- `MENU_DWELL_MS` is 1300.
- `GAZE_FRESH_MS` is 500.
- `LEAVE_MS` is 280.

There is no third dwell. There is no onset timer. The ring fills across the whole dwell: fill is `min(1, (now − dwellStart) / dwellMs)` from the moment the visit counts. At 0 ms the fill is 0. At 650 ms a play control commits. At 1300 ms a menu control commits. A 640 ms look does not commit. A 660 ms look does.

An `https` page does not open the socket and does not fall back to another host. Reconnect waits 2000 ms, and only if the check flag is absent and reconnect is not suppressed. On every open, send the nine hello bytes 1, 4, 0, 0, 0, 0, 5, 0, 0.

### Framing

A frame is a type byte, a little-endian unsigned 32-bit length, then the payload. A length above 1 MiB (1048576) resets the buffer and drops the chunk. An incomplete frame stays buffered.

- Type 1 with at least 56 bytes is a sample. Read a little-endian uint32 mask at offset 0. If `(mask & 64) === 0`, ignore the sample. If `(mask & 12) !== 0`, let left be the uint32 at offset 8 when `(mask & 4) !== 0`, otherwise 4, and let right be the uint32 at offset 12 when `(mask & 8) !== 0`, otherwise 4. If left and right are both non-zero, ignore the sample. The point is a little-endian float64 x at offset 40 and y at offset 48. Ignore the sample when either value is not finite or either is negative.
- Type 4 with length 0 clears the homography. Type 4 with a non-empty payload shorter than 72 bytes is ignored. Type 4 with at least 72 bytes is nine little-endian float64 values. If any is not finite, keep the previous homography. Otherwise store them.

Samples and the homography stay in memory. They are not logged, not put in `state()`, and not written into the page. Gaze bytes are numbers. They cannot become a symbol, a banner, or a cell id.

Warp, when a homography `h` is stored: `w = h[6] * x + h[7] * y + h[8]`. If `abs(w) < 1e-9`, use the raw point. Otherwise `x' = (h[0] * x + h[1] * y + h[2]) / w` and `y' = (h[3] * x + h[4] * y + h[5]) / w`. Multiply by the window size to get pixels. Each sample then moves 0.45 of the way from the previous smoothed point toward the new point. The first sample snaps.

A sample 500 ms old or older is stale (`now − gazeAt >= 500`). The ring hides, `dwell()` is 0, and the visit does not commit. A cell latch already made stays. During the check, `look` stamps freshness from the `step` clock, not from `performance.now()`. `look` sets `refreshHold`. While a test look is held and `refreshHold` is true, `step` sets `gazeAt` to `now` before the dwell math, so one 660 ms step still counts as fresh and can commit. The stale case sets `refreshHold` false and then steps 500 ms, which ages the sample without a new stamp.

### Pointer

One pointer at a time. A second contact is ignored until the first ends. While a pointer is down, gaze dwell does not advance and the ring fill stays at 0. The ring may still sit on the look. A pointer press clears a gaze visit that was in progress. It does not clear a cell latch. Releasing the pointer does not itself fire the button.

A click fires the button that receives the click, which is the control that got both the press and the release. Pressing one target and releasing on another does not fire the first target. `tap` and a finished look call that same button writer. `hold` and `release` do not.

### Play and menu

A button with `data-play="true"` dwells 650 ms. Every other button dwells 1300 ms.

Play, 650 ms: an empty cell, a box button when the board is in box mode, a live tray symbol and only while that symbol is a button, Undo, Hint, and Boxes.

Not targets: a given cell, a child-filled cell, a dim illegal symbol, the canvas, the banner, the Eyes chip, and the ring. Dim symbols are divs, not buttons, so they have no ring. Given cells and child-filled cells are divs, not buttons.

Menu, 1300 ms: the four band buttons, Let's go, Back to Menu, New board, Another, mute, and fullscreen. Another exists only after a solved board, and only while a second board is still allowed.

The latched cell's interior does not fill from the look. The selection is an ink outline, 4 px, `#2c2416`.

### Visit, leave, and latch

A visit fires once. Staying on the control after the commit holds the fill at 1 and does not fire again. Leaving and coming back is a new visit.

- If the look slips off the current control onto empty space, keep that visit for `LEAVE_MS` (280 ms) without advancing the fill. If the look returns to that same control in time, continue from the same `dwellStart`. At 280 ms off, discard the visit and set the fill to 0.
- If the look moves from one button to a different button, discard the old visit at once and start the new one at `now`.
- A stale sample, a pointer press, `releaseLook`, or the socket closing discards the in-progress visit at once. None of these clear a Sudoku cell latch.
- Latch on an empty cell: that cell stays selected after the eyes leave. A later empty cell replaces the latch. A second visit to the same cell does not clear it and does not write.
- Latch on a live symbol while a cell is latched: write it, clear the latch, and if the tray had replaced the grid, show the zoomed box again. No cheer unless the board is complete. The rest sentence is not a score.
- Latch on a live symbol with nothing latched: no write and no scold. On Tiny Tots the one tray symbol is live in `state().tray` before the first latch, and that early dwell does not write.
- Undo and Hint always dwell, even when their writer then no-ops.

### Two steps, and the third when the grid is large

The required order is cell, then symbol. On a 6×6 or 9×9 that fails the full-grid test below, the order is box, then cell, then symbol.

`view` is not a screen id. `go` is not involved.

- `flat`. The whole grid and the tray are on one screen. Always used for 4×4. Used for 6×6 and 9×9 only when every cell is at least 112 px and every tray symbol is at least 112 px on that same screen, and the stack fits in the height.
- `boxes`. Six box buttons in a 3×2, or nine in a 3×3. No tray yet.
- `zoom`. One box, enlarged. Empty cells in that box are buttons. The tray is on this screen only when its symbols stay at least 88 px on the short side and the cells stay at least 72 px. Prefer cells of at least 112 px when that still leaves the tray at least 88 px. If the tray would fall under 88, do not shrink it. Show the cells without a tray. Latching a cell then opens `symbols`.
- `symbols`. The grid is replaced by the symbol buttons until a symbol commits or the child dwells Boxes. Six symbols are 3×2. Nine symbols are 3×3.

Dwelling Boxes from `zoom` or from `symbols` returns to `boxes`, clears the enlarged box, and clears the latch so a symbol cannot write into a cell that is off screen.

### Sizes

Hard floor: every control's short side is at least 72 px. Never use 64 px. Comfort floor: 112 px for box buttons and for tray symbols, except the listed 4×4 cell size and the 72 px Undo and Hint height. A pretend viewport under 320 px wide does not fit. Do not shrink a target to pretend that it fits, and do not drop a column to make it fit.

Page pad for a flat grid is 8 px. Tray, box chooser, symbol screen, and Undo sit 16 px in from the screen edge. Menu chrome may sit 8 px in. Flat cells and a zoomed cell may use the 8 px pad, because a 16 px inset cannot hold the 4×4. Gap between tiled controls is 8 px, or 4 px only when an 8 px gap would push a cell under 72 px. Hit boxes match the painted controls. Do not inflate them.

`targetsAt(390, 844)` on a 4×4 reports cells of 87 px. The width formula is `floor((390 − 16 − 3 × 8) / 4) = 87`, from `4 × 87 + 3 × 8 + 16 = 388`. The tray is 2×2, each button `floor((390 − 32 − 8) / 2) = 175` by 112. Undo and Hint are 175 by 72. That stack, with the top chrome, fits in 844 px of height. It does not fit in 700 px of height together with a 112 px tray and a chrome row. On a live 390×700 window, keep the tray at 112 and Undo at 72, and drop the 4×4 cells to 72 px with 4 px gaps so the stack fits. Do not go under 72. `targetsAt` is what the family size case reads, and at height 844 it still reports 87.

At 1024×768, a stacked 4×4 of 112 px cells does not fit under the tray. Put the tray beside the grid when the width allows, and let the cells grow up to 112 px, not past it.

6×6 and 9×9 at 390 px wide do not get a full grid. A full 6×6 cell would be about 55 px, and a full 9×9 cell about 34 px. Both are under 72, so neither is painted. Use box buttons of 114 px: `(390 − 32 − 16) / 3 = 114`, inset 16 px, three columns. Six boxes use that width in a 3×2. Nine use it in a 3×3. The short side is 114, which clears 112.

Zoomed cells use the same three-column width when the box has three columns, so they are at least 72 and, at 390, 114. A 2×2 box on a wide window uses the larger cell that fits, still at least 72 and at least 112 when the tray can stay at 88 or more.

Symbol screen at 390: six symbols are 3×2 at 114 by 160. Nine symbols are 3×3 at 114 by 112. Both clear the 112 px comfort floor. The general width is `floor((width − 32 − 16) / 3)`.

`targetsAt(300, 600)` returns `fit: false` because the width is under 320 and a 4×4 cell is under 72. It does not clamp that cell up to 72.

`centers()` and `targetsAt` return live buttons only. Each target is `{ id, x, y, w, h, play }`. `centers()` also gives `nx` and `ny`, the center as a fraction of the real window. `targetsAt` returns `{ fit, targets }` and does not include givens, dim symbols, the banner, the ring, or Eyes.

## Generator

The functions are pure. They live in the page script. The same arguments return the same value. They do not read the clock, the DOM, the query string, the network, or `speechSynthesis`. There is no shared file.

`sudokuGenerate(band, size, seed)` returns `{ band, size, puzzle, solution, givens, fallbackUsed }`.

- `size` is 4, 6, or 9. `tiny` and `little` are size 4. `shop` is size 6. `rush` may be 6 or 9.
- `puzzle` and `solution` are row-major arrays of length `size * size`. Symbols are `1..size`. An empty puzzle cell is `0`. Row 0 is the top. Index `r * size + c`.
- `givens` is the count of non-zero puzzle cells.
- `fallbackUsed` is false when a dug grid passed, and true when a constant grid below was returned.

`sudokuCandidates(size, grid, index)` returns the ascending symbols from `1..size` that are not already in that cell's row, column, or box. A filled cell returns `[]`.

`sudokuCountSolutions(size, grid)` returns `0`, `1`, or `2`. It never counts past 2.

`sudokuSolveSingles(size, grid, mode)` returns a completed grid, or null. `mode` is `"naked"` or `"both"`.

`sudokuOpeningNaked(size, grid)` is true only when every empty cell currently has exactly one candidate. Placing them is not required for this predicate. A grid with no empties is true.

### Boxes

| Size | Box | Box index |
| --- | --- | --- |
| 4 | 2 rows by 2 columns | `floor(r / 2) * 2 + floor(c / 2)` |
| 6 | 2 rows by 3 columns | `floor(r / 2) * 2 + floor(c / 3)` |
| 9 | 3 rows by 3 columns | `floor(r / 3) * 3 + floor(c / 3)` |

`boxRows` is 2, 2, and 3. `boxCols` is 2, 3, and 3.

### Band table

| Band | Size | Givens | Floor | Technique | Extra |
| --- | --- | --- | --- | --- | --- |
| `tiny` | 4 | exactly 12 | 12 | Every empty is a naked single on the opening grid, all at once | At least 3 distinct symbols |
| `little` | 4 | 8 through 10 | 8 | Naked singles only, as a chain. A hidden single must not be required | At least 3 distinct symbols |
| `shop` | 6 | 24 through 28 | 24 | Naked singles, then hidden singles. No guess | At least 5 distinct symbols. Every row has at least 4 givens |
| `rush` | 6 | 18 through 22 | 18 | Same as shop | At least 5 distinct symbols |
| `rush` | 9 | 40 through 48 | 40 | Same as shop | At least 8 distinct symbols |

### Shared RNG

`makeRng(seed)` stores `state = seed >>> 0`. `rngUint(rng)` uses `Math.imul` and the JavaScript operators `^`, `|`, and `>>>`.

1. `state = (state + 0x6D2B79F5) >>> 0`.
2. `t = imul(state xor (state >>> 15), state or 1)`.
3. `t = (t xor (t + imul(t xor (t >>> 7), t or 61))) >>> 0`.
4. Return `(t xor (t >>> 14)) >>> 0`.

`rngInt(rng, lo, hi)` is `lo + (rngUint(rng) % (hi − lo + 1))`, both ends included.

`rngShuffle(rng, items)` copies the array and runs Fisher-Yates from the last index down to 1: swap `i` with `rngInt(rng, 0, i)`.

Seed 1 must yield these first four uints, in order: `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`.

`playSeed(clockMs, counter)` is `imul( (clockMs >>> 0) xor imul(counter >>> 0, 0x9E3779B9), 0x85EBCA6B ) >>> 0`. `playSeed(1, 1)` is `0x3B230FE8`. The pure generators never call `playSeed`. The page calls it only when `seed(n)` was not armed: `clockMs` is `Date.now()`, and `counter` starts at 1 and increases by 1 for each page generate. A pinned seed does not call `Date.now()`. Self-check always pins a seed. `engineSelfCheck` does not call `Date.now`.

### Completion

`fillComplete(size, rng)` starts from the base grid. The base value at row `r`, column `c` is `(boxCols * (r % boxRows) + floor(r / boxRows) + c) % size + 1`. That grid is a full valid sudoku. A broken completion is a bug, not a retry.

The base grids are:

4×4

```
1234
3412
2341
4123
```

6×6

```
123456
456123
234561
561234
345612
612345
```

9×9

```
123456789
456789123
789123456
234567891
567891234
891234567
345678912
678912345
912345678
```

`fillComplete` copies the base, then, in this order, on the same rng:

1. Shuffle the symbols `1..size`. Replace every value `v` with `shuffled[v − 1]`.
2. Inside each horizontal band of `boxRows` rows, top band first, shuffle those row indexes.
3. Shuffle the band order and apply it. The row that lands at position `r` is the row the permutation names.
4. Inside each vertical stack of `boxCols` columns, left stack first, shuffle those column indexes.
5. Shuffle the stack order and apply it the same way.

These moves preserve rows, columns, and boxes.

### Dig

One `makeRng(seed)` covers all 8 attempts. Do not reset it between attempts.

1. Repeat up to 8 times.
2. `solution = fillComplete(size, rng)`. Copy it into `puzzle`.
3. `order = rngShuffle` of the cell indexes `0 .. size * size − 1`.
4. Repeat while a pass still clears a cell.
   - If the number of givens is already at or below the floor, stop digging for this attempt.
   - Let `minC` be the smallest number of empties currently in any box. Keep this `minC` for the whole pass.
   - Walk `order` from the front. Skip a cell that is already empty. Skip a filled cell whose box does not currently have `minC` empties. Update that box's empty count as soon as a clear is kept, so the box is no longer at `minC` for the rest of the pass.
   - Before each trial clear, if givens are already at or below the floor, stop the walk and stop the passes. A pass must not delete a whole extra round of boxes and step past the floor. This is what keeps Rush 9 at 40 givens instead of falling through 45 to 36 and then failing the window.
   - Clear the cell on trial. Keep the clear only when all of the following hold. Otherwise put the symbol back and keep walking.
     - Distinct non-zero symbols still meet the band minimum.
     - Shop only: no row has more than 2 empties (every row still has at least 4 givens).
     - The band technique still accepts the grid: `sudokuOpeningNaked` for tiny; `sudokuSolveSingles(..., "naked")` equals `solution` for little; `sudokuSolveSingles(..., "both")` equals `solution` for shop and rush.
     - `sudokuCountSolutions` returns 1.
5. Accept the attempt when givens are inside the band window, the box spread holds, the distinct-symbol minimum holds, the shop row rule holds, the technique accepts the grid, and the solution count is 1. Otherwise start another attempt with the same rng.
6. If all 8 attempts fail, return the fallback for that band and size and set `fallbackUsed`. Do not dig again.

### Solvers

**Candidates.** As in `sudokuCandidates`.

**Count.** Recursive search. Each call counts as one node. Past 600 nodes, return 2, which rejects the removal. Pick the empty cell with the fewest candidates. A tie goes to the lowest index. Try those candidates in ascending order. An empty candidate list on that cell returns 0 for the branch. A full grid returns 1. Add the branches and stop the whole count at 2. Do not keep searching after the total reaches 2.

**Naked chain.** Scan indexes from 0. Place the first cell whose candidate list has length 1. Restart from the top. If no naked single remains and an empty remains, return null for mode `"naked"`.

**Hidden single, mode `"both"` only.** Units in this order: rows `0..size−1`, then columns `0..size−1`, then boxes `0..size−1`. Inside a unit, symbols `1..size`. A hidden single is a symbol that is missing from the unit and that has exactly one empty cell in that unit whose candidates include it. Place the first one found, then restart from naked singles. If a pass places nothing and an empty remains, return null.

Little Pals must finish in naked mode. Shop and Rush may use a hidden single. Neither may leave a cell for a guess.

### Fallback grids

These are the base completion with holes. `0` is empty. The solution is the base completion of that size, not a shuffled completion. Each grid already passes the band checks. `fallbackUsed` is ignored when the predicates are run on these constants.

Tiny, 12 givens:

```
1204
3012
2301
4023
```

Little, 8 givens:

```
1004
3002
2301
0003
```

Shop, 24 givens:

```
023056
450103
230560
501230
305610
602340
```

Rush 6×6, 18 givens:

```
020006
450103
200500
501230
305600
002340
```

Rush 9×9, 40 givens:

```
020400009
056009023
780123056
204007090
000801200
091230067
000608010
608000005
910340078
```

### Checks the generator must satisfy

For `sudokuGenerate(band, size, 1)` and for seed 2, on each row of the band table:

- `fallbackUsed` is false.
- `sudokuCountSolutions(puzzle)` is 1, and every non-zero puzzle cell equals `solution` at that index.
- The technique for that band accepts the grid. For tiny, `sudokuSolveSingles(puzzle, "naked")` also equals `solution`.
- Givens are inside the window and, for these two seeds, equal to the floor. Distinct symbols meet the minimum. Box empty-counts differ by at most 1.
- Tiny: `sudokuOpeningNaked` is true, and every empty cell's candidate list has length 1.
- Little: `sudokuSolveSingles(puzzle, "naked")` equals `solution`.
- Shop: every row has at least 4 non-zero cells. Mode `"both"` finishes. The solution count is 1, so no guess is required.
- Rush 9: at least 8 distinct digits and givens in 40 through 48. Seeds 1 and 2 have exactly 40.
- The five fallback grids pass the same predicates. Their solution is the base grid.

## Self-check cases

`selfCheck` drives time only through `step`, from 10000. It does not wait on a frame, does not open a socket, and does not treat wall-clock time as a pass or a fail. Assertions read `state()`, `centers()`, `targetsAt`, `dwell`, and `eyes`. The harness opens the file with `?selfcheck` plus one junk flag whose value looks like a URL or a tag. The real window used for hit testing should be at least 390 by 844 so the dwell cases can see a 4×4. `targetsAt` does not depend on that window.

Run the family cases first, then the Sudoku cases, then `engineSelfCheck`, and merge its failures. The storage assertion is collected after those later cases. `ok` is true only when `failures` is empty. Each failure is one sentence that starts with the case name.

`engineSelfCheck` returns `{ ok, failures }`. It does not call `Date.now`, `setTimeout`, `setInterval`, `requestAnimationFrame`, `fetch`, `WebSocket`, or `speechSynthesis`. It does not read the DOM. This page's `engineSelfCheck` runs only the three steps below. It does not generate a math problem, a word search, a flash-card plan, or a maze.

1. `rngUint` four times on `makeRng(1)`. Compare to `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`. Compare `playSeed(1, 1)` to `0x3B230FE8`. One `rngInt` on a fresh `makeRng(1)`, range `0..3`, stays inside that range.
2. `sudokuGenerate` for `(tiny, 4)`, `(little, 4)`, `(shop, 6)`, `(rush, 6)`, and `(rush, 9)`, seeds 1 and 2. Run the generator checks, including `sudokuCountSolutions`, `sudokuSolveSingles` or `sudokuOpeningNaked`, and the spread and distinct counts.
3. Run those same predicates on the five fallback grids.

### Family cases

At the start of `selfCheck`, snapshot `checking`, reconnect suppression, the socket, `localStorage` including the value of `pop-pals-v1`, and that the screen is the menu. Spy on `localStorage.setItem` and `removeItem` without calling them from the game. Spy on `speechSynthesis.speak` if that object exists.

1. `family-guards`. At the start, `checking` is true, reconnect is suppressed, and the socket is null.
2. `family-no-storage`. After the later cases, including the engine check, `localStorage` has no key for this game, `pop-pals-v1` is unchanged, and the page never called `setItem` or `removeItem`.
3. `family-play-dwell`. `seed(1)` and `start("tiny")`. On an empty cell, a look of 640 ms does not latch and does not change `placed`. A fresh look of 660 ms sets `latched` and does not change `placed`. `tap` on a different empty cell runs that same latch writer and still does not write a symbol. Use `releaseLook` between visits so the second visit starts clean.
4. `family-menu-dwell`. From the menu, a look on Let's go for 600 ms does not change the screen. A look for 1300 ms does, and the screen becomes the board for the highlighted band. From the board, a look on Back for 600 ms stays on the board. A look for 1300 ms calls `go("menu")`.
5. `family-pointer-blocks`. `hold` on an empty cell, then a 660 ms look, does not latch. `release`, then a fresh 660 ms look, does.
6. `family-stale`. A look that `step` ages past 500 ms, with `refreshHold` set false so the sample is not renewed, leaves `dwell()` at 0 and does not latch. A latch already made stays.
7. `family-target-size`. `targetsAt(390, 844)` and `targetsAt(1024, 768)` both report a short side of at least 72 for every target. On the 4×4 at 390×844 the cells are 87. `targetsAt(300, 600)` has `fit` false and does not clamp a cell up to 72.
8. `family-eyes`. `eyes()` has only the boolean keys `on` and `live`. A fresh test look sets `live` true and `on` true with the socket still closed. `#eyes` is shown for that look.
9. `family-mute`. The mute button's visible text is empty. It contains two inline speaker SVGs. The words Mute and Sound are not visible. `tap` sets `state().muted`. A second `tap` clears it.
10. `family-query-inert`. The junk flag's value is not in any element's text or any attribute. The snapshot at the start is still the menu. No socket was opened. Deep links did not run.
11. `family-plain-state`. `state()` can be turned into JSON and contains `level`, `screen`, `banner`, and `muted`.

### Sudoku cases

These run after the family cases and before the storage assertion. Pin `seed(1)` before each `start` unless the case says otherwise.

1. `unique`. A pinned board on each band has `solutions` 1. Also call `sudokuGenerate("rush", 9, 1)` so a narrow window still checks a 9×9.
2. `singles-only`. `singles` is true on the fresh board. Playing only `hint` plus `write` of the hinted symbol finishes the board and lands on rest. Cap the loop at `size * size` writes so a failed search still returns. Do this for each band.
3. `tiny-forced`. On the opening Tiny Tots board every empty cell has exactly one candidate, and `tray` has one entry whose `live` is true.
4. `little-range`. Givens are 8, 9, or 10 on a 4×4. `sudokuSolveSingles(4, puzzle, "naked")` equals `solution`. A hidden single is not required.
5. `shop-boxes`. The size is 6, givens are from 24 through 28, and every row has at least 4 clues. `sudokuSolveSingles(..., "both")` equals `solution`, and the solution count is 1. A guess is not required.
6. `symbol-bound`. The fresh 4×4 shows at least three distinct symbols, the fresh 6×6 at least five, and `sudokuGenerate("rush", 9, 1)` at least eight.
7. `latch-not-fill`. A 660 ms look on an empty cell sets `latched` and does not change `placed`. A 640 ms look does not latch. A 660 ms look on the live symbol then writes, and `latched` returns to null.
8. `given-ignores`. A 660 ms look on a given cell's rectangle leaves `latched` null. `latch` of that cell id does nothing.
9. `dead-symbol`. `write` of a symbol that is not a candidate, and `write` of a string, do not change the cell, do not create a mistakes field, and do not insert that string into the page.
10. `undo-child-only`. On a fresh board, `undo` does nothing. After one child write, `undo` removes that symbol and does not change a given.
11. `hint-does-not-write`. `hint` sets `hint` and `latched` and does not change `placed`. The hinted cell is still 0.
12. `another-gated`. `again` before `solved` does not change the board. After the board matches, `seed(2)` and Another deal the seed-2 grid, `placed` is empty, and the screen is the board. Finish that second board. Another is not offered, and `again` does not deal a third.
13. `rush-narrow`. `targetsAt(390, 844)` does not report a 9×9 of cells under 72 px. There are not 81 cell targets. Rush on that pretend viewport is a 6×6 of box buttons, or a 9×9 of box buttons, or a zoomed box whose cells are at least 72. Box buttons at 390 are 114.
14. `no-pencil`. No cell contains a stack of candidate buttons. No button is nested inside a cell button.
15. `no-tally`. `state()` has no mistakes, rank, clock, score, or lives field. A dead write leaves the banner without fail, wrong, or mistake.
16. `no-speech`. Nothing calls `speechSynthesis`. The spy on `speak`, if installed, stays at 0.

### Threats the check is backing

The socket host is pinned to `127.0.0.1` port 7081. Nothing concatenates a host, a query, or a payload onto it. There is no remote fallback.

No `innerHTML`, `insertAdjacentHTML`, or `document.write` of external data. External data means the query string, the socket payload, and the homography. Labels and the banner are authored strings assigned with `textContent`.

No `eval`, no `new Function`, and no timer that takes a string. The reconnect timer is a number of milliseconds.

Cell ids and symbols come from the fixed alphabet for that size. `write`, `latch`, and `zoom` of any other string do nothing and are not inserted into the page. The homography cannot become a symbol.

The generator cannot emit a played board with two solutions: a clear is kept only when the count is 1, and the fallbacks also count as 1. The 9×9 path is not painted as a full grid of cells under 72 px. `targetsAt(390, 844)` is the proof.

This file does not reference `speechSynthesis`.

## README paragraph

Add this bullet to the Play list in `/home/joseph/code/games/README.md`, after Saddle Up! and before Add a game:

- [Sudoku](https://calledtoconstruct.github.io/games/sudoku/)

Add this section in that same place, after the Saddle Up! section and before Add a game. Plain sentences. Do not mention the check flag, the window hook, or a score.

```
## Sudoku

A picture and digit board for ages 2 and up. Look at an empty square until it holds, then look at the symbol that belongs there. The first look does not fill the square. Nothing is timed, and nothing counts mistakes. Tiny Tots play four pictures on a 4 by 4 board that starts with 12 of the 16 squares filled, and each empty square has only one picture that fits. Little Pals use the same four pictures on a 4 by 4 board that starts with 8, 9, or 10 squares filled. Shopkeeper uses the digits 1 through 6 on a 6 by 6 board that starts with 24 to 28 squares filled. Rush uses those digits on a 6 by 6 board that starts with 18 to 22 squares filled, or the digits 1 through 9 on a 9 by 9 board that starts with 40 to 48 filled, and only when the squares stay large enough. Another deals one more board after a finished one, and then the sitting stops.

Open `sudoku/index.html` in a browser, or play [Sudoku](https://calledtoconstruct.github.io/games/sudoku/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an empty square, a box, a tray symbol, Undo, or Hint until the ring fills. That look takes 650 ms. Menu buttons take twice that, so reading one does not press it. Mouse and touch work in the same board. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.
```

The license stays the repo MIT license. Copyright 2026 Joseph Woolley. This game file does not add a second license or a different copyright.

## Key Decisions

- The page clock is the architecture memo. Play dwell is 650 ms and menu dwell is 1300 ms, measured across the whole visit. The ring fill is `(now − dwellStart) / dwellMs`. The gaze memo's 200 ms onset, its fill formula `(t − 200) / (D − 200)`, and its rule that a leave cancels at once are not used. `LEAVE_MS` is 280. It is only the grace for an unfinished visit that slips onto empty space. A finished cell latch is not cleared by leaving, by a stale sample, by a pointer press, or by the socket closing. Family dwell proof is 640 ms does not commit and 660 ms does.
- Gaze wins on which controls are play or menu, and on what a miss does. Empty cells, boxes, live symbols, Undo, Hint, and Boxes are play. Bands, Let's go, Back, New board, Another, mute, and fullscreen are menu. A miss is a no-op. There is no illegal write, no scold, and no reset.
- Generator numbers win when a count differs. Rush 9×9 givens are 40 through 48, not an open-ended "at least 40". Shopkeeper also requires at least 4 givens in every row. Seeds 1 and 2 land on the floor. The floor is checked before every trial clear, not once per pass. A pass that only checked once would delete one more cell from every box and step a Rush 9×9 from 45 givens to 36, outside the window, and seeds 1 and 2 would return the fallback. That reading is rejected.
- A full 9×9 is painted only when every cell and every tray symbol is at least 112 px. The engine and the architecture memo hide a 9×9 when a full-grid cell would be under 72 px, and the page then generates a 6×6. Between those floors the page keeps the generated puzzle and shows box, then cell, then symbol. `targetsAt(390, 844)` never lists 81 cells under 72 px.
- The 4×4 cell size of 87 px is the width formula at 390, and `targetsAt(390, 844)` reports it. A live 390×700 window cannot hold 87 px cells plus a 2×2 tray of 112 px plus a 72 px Undo row under the chrome. There the cells drop to the 72 px hard floor and the tray stays at 112. They do not drop to 64.
- Another deals the next grid but does not call the sitting reset inside `start`. `start` sets `dealt` to 1. `again` deals board 2 only after a solved board. That is how the age ceiling (one optional Another, then stop; Shopkeeper and Rush also stop at 2) survives the architecture rule that Another deals again.
- Tiny Tots marks exactly one tray symbol live on the opening board so the forced-tray check is true before the first latch. Dwelling it early does not write. Older bands keep illegal symbols visible, dim, and not buttons.
- `engineSelfCheck` in this file is the Sudoku prefix of the engines memo: the RNG vectors, the five generates for seeds 1 and 2, and the five fallback grids. The other four games are not in this file.
- The gaze client is copied into this file. It is not imported. There is no `gaze.js` and no shared folder.

## Out of scope

- A second file, a bundler, a shared gaze module, or an asset folder.
- Pencil marks, candidate stacks inside a cell, a 0–9 keypad, and a typed answer.
- A clock, a countdown, a mistake tally, a life, a buzzer, a score, a streak, a star rating, or a rank.
- Speech, `speechSynthesis`, and recorded audio.
- `localStorage`, cookies, beacons, and `fetch`.
- Remote pictures, fonts, scripts, or symbol sets.
- Blink, wink, and a drag that commits.
- A 9×9 of cells under 72 px, a 17-clue board, dancing links, and digging for a minimal puzzle.
- A fifth band, a renamed band, or dropping Rush.
- An onset timer, a third dwell, and a per-game rewrite of the hello bytes or of the one-pointer rule.
- Hint that writes the symbol by itself. Another before the board matches. A third board in one sitting.
- Editing Pop Pals, Sugar Counter, Sun Garden, or Saddle Up!.

# Maze

**Date.** 2026-10-18. Dots revised 2026-10-19. Branches revised 2026-10-19. Clearing revised 2026-10-19.

**Status.** Build spec. One file, `maze/index.html`. Hook `window.maze`. No second script, no module, no asset folder. The 2026-10-19 dots note below replaces the unlit gold disks, the blue token disk, the off-grid cluster of lit buttons, and the rule that a lit stone is never under 112 px. The 2026-10-19 branches note replaces the straight-spur rule and the line that the longest corridor is the way out. The banner string stays "Look at the next stone." Solution length, one route, the lit cap on Little Pals and older, and the dwell times stay.

**Audience.** One child, ages 2 and up. Mouse, touch, and gaze do the same acts. The implementer should not have to open another note to choose a clock, a grid, or a walk.

**Dots revised 2026-10-19.** The canvas does not paint a dot on every stone. A blue meeple stands on the cell where the player is. HTML buttons exist only for the cells `mazeLit` returns: the next corner, junction, or lane end the player can walk to, including the way back and a dead-end leaf, and the goal when that is the next stone. Tiny Tots still lights only the next cell. Each dot is centered on that cell and sized to the lane (`max(18, min(floor(pitch × 0.46), floor(pitch − 8)))`). A dot may be under 72 px. Chrome and menu buttons stay at least 72 px. A width under 320 still does not fit. The menu line is "Look at the next dot. The meeple walks there." `aria-label` is "Next dot", or "Goal" on the goal.

**Branches revised 2026-10-19.** This replaces the straight-spur rule, the line that the longest corridor is the way out, the line that an attachment lights the dead-end leaf in one step, and the dots note's line that Tiny Tots lights only the next cell. Little Pals, Shopkeeper, and Rush each grow one wrong path that bends and is longer, in open cells, than the remaining way to the exit. The player meets that fork before the maze is over. Other wrong paths stay shorter than the way out, so a long straight stub is not the hint. Rush still has at least two wrong paths of three or more cells. A wrong path's corners are stones. The meeple walks those bends one at a time and has to come back. A straight run is still one step. Tiny Tots stays a straight row on some deals. On a 3×3, some deals add one bent wrong turn the child can enter and leave. A fresh maze still lights one dot. At a Tiny fork, two dots can show. The First Steps menu line is "Ages 2 to 3. A short path. Sometimes a wrong turn." The four maze names are First Steps, Hedge Row, Twisty, and Long Trail. The four fallback mazes stay the earlier straight examples and are used only when a seed cannot be built. The banner string stays "Look at the next stone."

**Clearing revised 2026-10-19.** After the solution and its wrong paths are placed, Hedge Row, Twisty, and Long Trail keep carving corridors into the empty cells of that same rectangle. A fresh maze may not leave an empty block larger than one cell. The added corridors are still dead ends: one route remains, a bent wrong path is still longer than the way out, and a straight wrong path still is not. First Steps is unchanged. The fallback mazes are unchanged. The Maze paragraph in the README matches this note.

## Player loop

The page opens on the menu. The child, or a parent, highlights a band and dwells Let's go. The page deals one maze and shows it.

A gold stone that is lit is the only play target. The child dwells it. The blue token walks the open corridor to that stone. The walk may pass straight floor cells. It does not pass another stone, it does not cross a wall, and it does not cut a corner. The token's logical cell updates inside that commit, before the function returns. A slide along the corridor is paint only.

The next stones then light. A fresh maze lights exactly one stone, the first step off the start, on every band. A corner lights the stones on either side of the bend. A spur attachment lights three: back, onward, and the dead-end leaf. Little Pals lights that third stone. Hiding it would delete the dead end or trap the token.

Walking into a dead end is allowed. The maze does not reset. The stone behind the token lights. The child dwells it and walks back. Walls, floor, the token, an unlit stone, a stone two forks away, and the trail are not targets. A look at them changes nothing. There is no buzzer, no lost life, no "wrong," and no return to the start.

Dwelling the goal stone walks onto it and opens the rest card. The banner says "You made it." It does not say lose, fail, or wrong. It shows no time and no count. Another, while the sitting is under its ceiling, deals the next maze of the same band. After the ceiling, Another is gone. Back to Menu returns to the band picker and does not punish.

There is no round timer, no lives, no score, and no speech.

## Bands

Parent names stay Tiny Tots, Little Pals, Shopkeeper, and Rush. Ids are `tiny`, `little`, `shop`, and `rush`. Do not rename them. Do not add a band. Do not drop Rush.

`4×4` is not a Little Pals grid. Co-lit stones there fail a Chebyshev distance of 2 inside the try cap. Rush does not use `8×8`, for that reason and because the long-spur quota is missed. Shopkeeper does not use `7×7`. Tiny Tots is not a forked maze.

Solution length is the number of dwells after straight runs collapse. A corner stays a stone. A straight degree-2 cell is the inside of a corridor and is not a stone, except on Tiny Tots. Tiny Tots does not compress: every open cell is a stone, and the promised length is the number of steps between cells.

| Band | Grid | Solution dwells | Dead ends | Lit |
| --- | --- | --- | --- | --- |
| `tiny` | a straight `1 × N`, or a simple path on a `3×3` | 2, 3, or 4 | 0 | exactly 1, the next cell toward the goal, until the goal |
| `little` | `5×5` only | 4 through 7 | 1 or 2, each length 1 | every stone-neighbor, and never more than 3 |
| `shop` | `6×6`, then `8×8` | 8 through 14 | 3 through 6, each length 1 through 3 | at most 3 |
| `rush` | `10×10`, then `12×12` | 12 through 20 | 6 through 12, each length 1 through 5, and at least 2 of those lengths are 3, 4, or 5 | at most 3 |

Dead-end length is the number of open cells from the branch to the leaf, not counting the branch. A one-cell spur has length 1.

The graph is a tree: one connected component, and `edges.length === cells.length - 1`. Routes = 1. Sidewinder, the binary-tree algorithm, braided loops, and a full-rectangle growing tree are not this builder. A full-rectangle growing tree grows dead ends of many cells and cannot hit the table.

Sitting ceiling, counted in finished mazes, not in a clock:

| Band | Mazes in one sitting |
| --- | --- |
| `tiny` | 1, then one optional Another, then stop. Ceiling 2. |
| `little` | 1, then one optional Another, then stop. Ceiling 2. |
| `shop` | 2 |
| `rush` | 2 |

The ceiling is a maximum. Back to Menu may end the sitting on the first maze. Nothing forces the second maze. Caps are not quotas and are not scores.

What each band is on the menu, as the small line under the button:

- Tiny Tots. Ages 2 to 3. One stone lights.
- Little Pals. Ages 4 to 5. The next stones light. Up to three.
- Shopkeeper. Ages 6 to 8. Up to three stones light.
- Rush. Ages 9 and up. Up to three stones light. A longer walk.

Do not put a grid size on the button.

## Screens and API

### File

`maze/index.html` only, plus the Maze section of `/home/joseph/code/games/README.md` specified below. The license stays the repo MIT license. Copyright 2026 Joseph Woolley. This file adds no second license.

The head holds charset, a viewport meta (`width=device-width, initial-scale=1, viewport-fit=cover`), a cream theme color `#f4f0d8`, and the title `Maze`. Nothing else is loaded. No frame, no remote image, font, style sheet, script, or beacon. No `fetch`. The script does not call `speechSynthesis`. The script does not read or write `localStorage`, does not set a cookie, and does not send a beacon.

Do not use `innerHTML`, `insertAdjacentHTML`, `document.write`, `eval`, or `new Function`. Do not start a timer with a string. Labels and the banner are authored strings assigned with `textContent`. Gaze bytes are numbers. The raw query string is never written into text or an attribute.

Font stack: Arial Rounded MT Bold, then Trebuchet MS, then Segoe UI, then sans-serif. Ink `#2c2416`. Paper `#f4f0d8`. Card `#fffdf6`. Sun-gold `#ffc857` with a pressed edge `#e2a62e`.

### Body, in this order

1. `#view`, a full-page canvas used only to draw. `pointer-events: none`. It is never the gaze hit.
2. Chrome that is always in the tree: `#mute`, `#full`, `#back`, `#eyes`, `#gaze`, and `#banner`.
3. `#screen-menu`.
4. `#play`, the host for lit-stone buttons.
5. `#screen-rest`.

The self-check node is not in the authored markup. The loader creates a `pre` with id `selfcheck-result` only when `selfcheck` is present.

`#mute` is a button at least 72 by 72 px. It contains two inline speaker SVGs, one hidden at a time: a speaker with sound marks, and the same speaker with a slash. It has no visible text Mute or Sound. `aria-label` is "Mute sound" when sound is on, and "Turn sound on" when muted. `#full` is a button of height 72 px and min-width 72 px, label `Fullscreen`. `#back` says `Back to Menu`, height 72 px, min-width 72 px, and is hidden on the menu. `#banner` is one short sentence, `pointer-events: none`, and it is not a score, a timer, or a rank. `#eyes` is a chip, not a button, `pointer-events: none`, text `Eyes`. `#gaze` is the ring, not a button, `pointer-events: none`, `z-index: 60`.

### Menu

`#screen-menu` holds the title `Maze`, the line `Look at the next stone. The token walks there.`, the line `Ages 2 and up`, four band buttons, and `#lets-go` labeled `Let's go`.

Band button ids are `tiny`, `little`, `shop`, and `rush`. The visible name is Tiny Tots, Little Pals, Shopkeeper, or Rush, with the small line from Bands. The highlighted band has `aria-pressed="true"`. Default highlight is `tiny`. A band button only changes the highlight. It does not deal a maze. Let's go and the band buttons are menu dwells. They do not carry `data-play`.

### Maze screen

`go("maze")` shows the canvas maze and one button per lit stone inside `#play`. It hides the menu and the rest card. It shows Back to Menu.

### Rest

`#screen-rest` holds one calm sentence, `You made it.`, and `#another` labeled `Another` while the sitting is under the ceiling. Another is a menu dwell. It does not carry `data-play`. It is not on the maze screen and it is not shown as a punishment. When two mazes of this sitting are already finished, the Another button is not in the tree. The rest card shows no grade, no miss count, no time, and no rank. Back is the same `#back`.

### Screen writer

Screens are `menu`, `maze`, and `rest`. `go(screen)` is the only writer of the screen id. An unknown id does nothing. Other helpers may call `go`. They may not assign the screen themselves. `go` shows one screen and hides the others. Back always calls `go("menu")`. Reaching the goal calls `go("rest")`. None of these paths write storage.

Banner strings, exact:

| Screen | Banner |
| --- | --- |
| `menu` | `Pick who is playing.` |
| `maze` | `Look at the next stone.` |
| `rest` | `You made it.` |

The banner never contains lose, fail, wrong, a move count, or a time.

### Hook

Assign `window.maze` as soon as the script runs, before the query is handled. `render` runs before any hook function returns, including while the check flag is on and the frame loop is off.

- `start(band)` sets the band when `band` is one of the four ids. An unknown band does nothing. It resets the sitting (`finished` returns to 0), deals maze 1, and calls `go("maze")`. It does not open the socket.
- `go(screen)` as above.
- `look(nx, ny)` places a test gaze at window fractions. It does not commit.
- `releaseLook()` drops the test gaze, hides the ring, and hides Eyes.
- `hold(nx, ny)` is a pointer down. A second hold is ignored until `release`.
- `release()` is the matching pointer up. It does not fire a button.
- `tap(nx, ny)` runs the writer for the button at that point, with no dwell, and does not leave the pointer down.
- `step(now)` is the only clock.
- `dwell()` returns the ring fill from 0 to 1.
- `eyes()` returns `{ on, live }` and nothing else. No coordinates.
- `centers()` returns window fractions and pixel boxes for every live target.
- `targetsAt(w, h)` returns those boxes for a pretend viewport and does not resize the real window.
- `state()` returns plain data.
- `seed(n)` pins the next generate only. It does not change a maze already on screen. It is not read from the query.
- `move(stoneId)` is the only writer of the token's stone.
- `selfCheck()` returns `{ ok, failures }`. Each failure is one sentence naming the case.

`start`, a band highlight plus Let's go, and a deep link share one band writer. A lit-stone button, `tap` on that button, and a committed look on that button share `move`. Let's go's writer is `start` of the highlighted band.

`state()` always includes `level`, `screen`, `banner`, and `muted`. It also includes:

| Field | Meaning |
| --- | --- |
| `level` | `tiny`, `little`, `shop`, or `rush`. On the menu, the highlighted band. Default `tiny`. |
| `screen` | `menu`, `maze`, or `rest`. |
| `banner` | The sentence in the table. |
| `muted` | Boolean. |
| `rows`, `cols` | Grid size of the current maze, or `0` before the first deal. |
| `stones` | Every stone, in solution order, then dead-end leaves in attachment order. Each entry is `{ id, lit, goal }`. `id` is `"r,c"`. `lit` is whether `mazeLit` of the token contains it. `goal` is true only for the goal. |
| `token` | Stone id where the token is, or `null` before the first deal. |
| `moves` | Successful `move` calls on this maze. Starts at 0 on each deal. A rejected `move` does not increment it. Not painted. Not a score. |
| `deadEnds` | How many dead-end leaves the maze has. A number. Tiny is `0`. It does not increase when the child walks in. |
| `solution` | The promised dwell count, an integer: `mazeSolution(maze).length - 1`. Tiny is 2, 3, or 4. Not a list of ids. |
| `routes` | `1`. |

`state()` never includes a DOM node, a gaze coordinate, the socket URL, the query string, `msLeft`, a best time, lives, a score, wrongs, a rank, or a speech field. It must survive `JSON.stringify`.

Before the first deal, `rows` and `cols` are 0, `stones` is `[]`, `token` is `null`, `moves` is 0, `deadEnds` is 0, `solution` is 0, and `routes` is 1.

### `move`

`move(stoneId)` is the only writer of the token.

- If `stoneId` is not in `mazeLit(maze, token)`, return `null`. Do not change `token`, `moves`, the screen, or the banner. Do not insert `stoneId` into the page.
- That includes an unknown string, an unlit stone, a wall, the floor, and a stone two forks away. `mazeWalk` to a non-neighbor returns `null`, and `move` then returns `null` and does not move the token.
- If it is lit, set `token` to that id immediately, increment `moves`, recompute which stones are lit, and return the cell-id list from `mazeWalk` (both ends included).
- If that id is the goal, call `go("rest")` after the token update and still return the list. `finished` increases by 1.
- A dead-end leaf is a legal destination. Stay on `maze`. Do not set `token` back to the start.

On Tiny Tots the cell behind the token is a graph neighbor but it is not lit. `move` to it returns `null`. The child only walks toward the goal.

### Sitting and Another

`finished` counts mazes already brought to the goal in this sitting.

- `start(band)` sets `finished` to 0, then deals. Let's go and the deep links use `start`.
- The goal sets `finished` to `finished + 1` and calls `go("rest")`.
- Another is shown only when `finished` is 1 and the ceiling is 2. Every band's ceiling is 2.
- Another deals a new maze of the same band and calls `go("maze")`. It does not set `finished` back to 0.
- When `finished` is 2, Another is absent. A call that would deal a third maze does nothing.
- Back to Menu does not change `finished`. The next Let's go calls `start` and the sitting begins again.

`start` resets the sitting. Another must not call that reset, or the ceiling would never stick. Both paths use the same generator and the same `go`.

### Seed

`makeRng`, `rngInt`, `rngShuffle`, and `playSeed` are defined in Generator. The pure generator never calls `playSeed` and never reads the clock.

`seed(n)` stores `n >>> 0` as a one-shot pin. The next `mazeGenerate` from the page uses it, then clears the pin. A sitting already on screen is unchanged.

When no pin is stored and the page is not in the check, the seed is `playSeed(Date.now(), counter)`. `counter` starts at 1 and increases by 1 after every page generate, pinned or not. When the check is running, an unpinned page generate uses seed `1` and does not call `Date.now`. `engineSelfCheck` passes seeds `1` and `2` itself and does not call `Date.now`.

### Deep links

Read the query before `requestAnimationFrame` and before `connectGaze`. Flags are presence checks. The value is ignored. Unknown flags do nothing. No flag changes the socket URL.

Apply deep links only when `selfcheck` is absent. If `rush` is present, call `start("rush")`. Otherwise if `play` is present, call `start("tiny")`. One start, not two.

### Boot when `selfcheck` is present

Set `checking` and `suppressReconnect`. Call `selfCheck`. Create `#selfcheck-result`, set its `textContent` to `JSON.stringify` of `{ ok, failures }`, and set `document.title` to `SELFCHECK_OK` or `SELFCHECK_FAIL`. Then stop. Do not call `requestAnimationFrame`. Do not construct a `WebSocket`. Do not apply deep links. If a socket already existed, close it and keep reconnect suppressed. The whole check is synchronous. No `setTimeout`, no `setInterval`, no wait, no `requestAnimationFrame`. `performance.now()` does not decide a dwell during the check. Time moves only because the check calls `step`.

### Paint

The canvas draws walls, floor, unlit stones, the token, the goal ring when the goal is not the lit button, and the trail. Lit stones are HTML buttons, and those buttons are the painted lit stones. Do not paint a second gold disk on the canvas for a lit cell.

Colors, exact:

| Thing | Paint |
| --- | --- |
| Wall | `#4a2c14`, thickness `max(8, pitch * 0.22)` |
| Floor | `#fff8e8` |
| Unlit stone | Gold disk `#c4a15a`, diameter half the cell's floor width, at least 4 px. No label, no face, no letter. Not a button. May be under 112 px. |
| Lit stone | Button, gold disk `#ffc857`, rim `#e2a62e`. No text. `aria-label` `Stone`, or `Goal` when it is the goal. |
| Lit goal | That same button plus a hollow ring `#1f8a4c`, about 6 px of stroke. |
| Unlit goal | The same hollow ring on the canvas, not a button. |
| Token | Blue disk `#2c6ecb` on the canvas, radius about 0.28 of the cell pitch, centered on the token cell. Not a button. |
| Trail | Stroke `rgba(44, 110, 203, 0.35)`, width about 0.25 of the pitch, along cell centers. |

The open grid is centered in the playfield. The playfield starts 8 px in from the sides and 8 px above the bottom. Its top is below the chrome row (the 72 px buttons sit 10 px from the top, so the playfield top is 92 px). Cell pitch is the largest square that fits `rows` by `cols` in that rectangle. A missing edge is a wall. The outer rim is a wall. Cells that are not in `cells` are wall.

### Where lit buttons sit

Hit boxes match the painted buttons. Padding around a stone is 0. The button box is a square of side `D`. The disk is inscribed in that square. `D` is never under 112. Never shrink a stone to pass a size check.

Let the usable center area inset the button by 16 px from the viewport edge, and keep it below the chrome. A layout **fits at comfort** when every lit center can sit in that area with pairwise center distance at least `2 * D` (one full stone-width of floor between the edges).

1. If one lit stone fits at `D = 160` with a 16 px inset, use 160. A single stone has no pair, so this is just "the disk fits."
2. Else if the lit set fits at comfort with `D = 160`, use 160.
3. Else if it fits at comfort with `D = 112`, use 112.
4. Else keep `D = 112` and reduce the gap, but not below 8 px of dead floor between the squares (center distance at least `112 + 8`). If that relaxed placement does not fit, `targetsAt` for that width and height returns `{ ok: false, targets: [] }`. The live page still does not paint a stone under 112.

At 390 px wide, three 160 px stones cannot keep a 320 px center gap. Use 112. Three 112 px stones can keep a 224 px center gap in a 390 × 700 window by using a triangle, not a row. Do not drop a lit neighbor to make the gap. Every stone neighbor stays a button.

Place the buttons in this order. Sort the lit ids by row, then by column. Call them A, B, and C.

- One stone: center of the usable area.
- Two stones: A on the left, B on the right, pair centered. Separation is `2 * D` when that fits, otherwise the relaxed gap.
- Three stones: A at the top, B at the bottom left, C at the bottom right. Horizontal separation and vertical separation follow the same gap rule. Center the triangle.

`data-play="true"` is set on lit-stone buttons only. The button id is `stone-R-C`. The stone id lives in `data-stone="R,C"` and is what `move` receives. Unlit stones are not buttons.

### Token slide

If `checking` is false and `prefers-reduced-motion: reduce` does not match, the canvas may slide the disk along the returned polyline over 400 ms of `step` time. `state().token` is already the destination before the slide starts. The slide does not block input, does not commit, and does not change state when it ends. During the check, or when reduced motion matches, snap the disk to the token cell. Reduced motion still commits the move.

### Sound

One sine blip on a successful `move`, including a dead end and including the goal. About 520 Hz, about 90 ms, heard through a master gain of 0.5. Muted master gain is 0. No second sound for a dead end. No sound on a rejected look, a menu dwell, or a miss, because a miss does not happen. Create the `AudioContext` on the first pointer press or the first committed gaze action, not at load and not during the check. No audio file. Mute does not remove buttons.

### Threats

The socket URL is the literal `ws://127.0.0.1:7081`. Nothing concatenates a host, a query, or a payload onto it. There is no remote fallback. Stone ids come from the generated graph. `move` of any other string returns `null` and is not written into the page. The homography cannot become a label. A failed generate returns the fallback maze in this spec. It does not draw two routes and it does not draw lit stones whose grid neighbors sit at Chebyshev distance under 2.

## Gaze targets

Copy this client into the file. Do not share it in a second file.

### Constants

Spell them exactly:

- `GAZE_URL` is `ws://127.0.0.1:7081`
- `PLAY_DWELL_MS` is 650
- `MENU_DWELL_MS` is 1300
- `GAZE_FRESH_MS` is 500
- `LEAVE_MS` is 280

There is no third dwell. There is no onset added on top of 650. The ring fills across the whole dwell. A 640 ms look does not commit. A 660 ms look does. `LEAVE_MS` is declared so the gaze block carries the same names as the other games. Maze does not wait 280 ms before cancelling a look, and it does not add 280 ms to a commit. Leaving a target drops that dwell on the next `step`. Adding a grace would be a second timer.

### Who dwells

Every selectable thing is a real `button`. Hit testing is `elementFromPoint`, then the nearest `button`. The canvas, the ring, the banner, the trail, and Eyes are not buttons.

| Control | `data-play` | Dwell |
| --- | --- | --- |
| Lit stone | `true` | 650 ms |
| Band, Let's go, Back to Menu, Another, Mute, Fullscreen | absent | 1300 ms |

Play targets are lit stones and nothing else in the maze. Not targets: walls, floor, the token, an unlit stone, a stone two forks away, the trail. Menu targets are the band buttons, Let's go, Back to Menu, Another, Mute, and Fullscreen. Eyes is not a control.

A click or a tap on a live target is the same writer as a finished dwell, with a wait of 0 ms. It is one step, not a drag. `hold` followed by `release` does not fire. A press that goes down on one button and up on another does not call `move`. While a pointer is down, gaze dwell does not advance and the ring fill stays at 0. The ring may still sit on the look. A pointer press clears a gaze dwell that was in progress. A second contact is ignored until the first pointer is up.

### Clock

`step(now)` stores `now`. Dwell elapsed is `now - dwellStart`. Fill is `min(1, elapsed / ms)` while the pointer is on a live button and the sample is fresh. At the entry timestamp the fill is 0. At 650 ms on a stone the fill is 1 and `move` runs once. At 1300 ms on a menu button the fill is 1 and that button runs once. Staying does not fire again. The fill is not `(t - 200) / (D - 200)`. There is no 200 ms silent onset.

A look of 600 ms on Let's go or Back does not change the screen. A look of 1300 ms does. A look of 640 ms on a lit stone does not change `token`. A look of 660 ms does.

Switching buttons resets `dwellStart`. The previous button does not commit.

Cancel, with no move and with the trail cleared:

- The next `step` is off that button, including a move onto the floor.
- The sample is 500 ms old or older (`now - gazeAt >= 500`).
- A pointer goes down.
- The socket closes.

Those clear the ring only. They do not move the token and they do not reset the maze. There is no persistent latch.

`family-stale` needs a sample that is old while a test look is still stored. A held test look normally refreshes `gazeAt` to `now` at the start of `step`, which is why one step from the entry time `T` to `T + 660` still commits. The check may turn that refresh off for one step, the way a held look stops being a new sample. With the refresh off, `step` 500 ms past the last stamp leaves `dwell()` at 0 and does not commit. Turn the refresh back on after that assertion. Real gaze uses the same age rule: no new sample for 500 ms, and the ring dies.

During the check, `look` stamps freshness from the `step` clock. The check's first `step` is `step(10000)`. `performance.now()` is not the dwell clock. Outside the check, the frame loop calls `step(performance.now())`.

Smoothing matches the shipped horse game: the first sample snaps to the point, and each later sample moves 0.45 of the way toward the new point. `look` during the check snaps, so the test point lands on the button.

### Ring and Eyes

The ring is 54 px, centered on the pointer. Fill is green `#2f8a3e` over paper `rgba(244, 240, 216, 0.45)`. It ignores pointer events. It is shown only while the sample is fresher than 500 ms. Off a live button the fill stays 0. A stale look hides it and forces `dwell()` to 0.

`eyes().on` is true when the socket is open or a test look is held. `eyes().live` is true when `on` is true and the sample is fresher than 500 ms. The Eyes chip is shown exactly when `on` is true. It is hidden otherwise. The chip is not a target.

### Trail

The trail may draw after the look has rested. Rested means the dwell fill on that lit stone is greater than 0 and the dwell has not committed. That uses the same `dwellStart` as the ring. It is not a second clock, not an onset, and not a commit. The entry step has fill 0, so it draws no trail. The next `step` with elapsed time draws it.

The trail follows `mazeWalk` from the token to that stone, orthogonal cell by cell. It does not cut a corner. It is not a hit area and it is not in `centers()` or `targetsAt`. Leave, cancel, pointer down, or commit clears it. `prefers-reduced-motion: reduce` skips the trail and still commits.

### Socket

An `https` page does not open the socket and does not fall back to another host. `file://` and `http://` may open it. On every open, send the nine hello bytes `1, 4, 0, 0, 0, 0, 5, 0, 0`. Reconnect waits 2000 ms, and only if the check flag is absent and reconnect is not suppressed. If the socket is down, hide the ring, retry, and let mouse and touch play the whole game with no wait and no missing control.

Framing is a type byte, a little-endian unsigned 32-bit length, then the payload. A length above 1 MiB (1048576) resets the buffer. Type 1 with at least 56 bytes is a sample. Type 4 with length 0 clears the homography. A non-empty type 4 shorter than 72 bytes is ignored. Type 4 with at least 72 bytes is nine little-endian IEEE-754 binary64 values. Samples and the homography stay in memory. They are not logged, not put in `state()`, and not written into the page.

Sample, little-endian: a uint32 mask at offset 0. If bit 64 is clear, ignore the sample. If bits 2 or 3 of the mask's low byte are set (`mask & 12`), read a uint32 at offset 8 when bit 2 is set and a uint32 at offset 12 when bit 3 is set. A missing eye bit is treated as the value 4. If both eye values are non-zero, ignore the sample. The point is a binary64 x at offset 40 and y at offset 48. Ignore it unless both are finite and both are `>= 0`. Warp that point, then multiply by `innerWidth` and `innerHeight`, then smooth.

Warp is a 3×3 matrix, row-major, in those nine floats `h0` through `h8`. Let `w = h6 * x + h7 * y + h8`. If `abs(w) < 1e-9`, keep the unwarped point. Otherwise `x' = (h0 * x + h1 * y + h2) / w` and `y' = (h3 * x + h4 * y + h5) / w`. No homography means the identity.

### Size

Hard floor: every real control's short side is at least 72 px at a 390 × 844 pretend viewport and at 1024 × 768. Lit stones use the 112 or 160 rule above, so they clear 72. A pretend viewport under 320 px wide does not shrink targets. `targetsAt(300, 600)` returns `{ ok: false, targets: [] }`. The game does not pretend the targets fit.

`targetsAt(w, h)` when `w >= 320` returns `{ ok: true, targets: [...] }` for the targets of the **current screen**, laid out as if the viewport were `w` by `h`. Each target is `{ id, x, y, w, h }`. `x` and `y` are the top-left of the button box. Every box has `min(w, h) >= 72`. Calling `targetsAt` does not move the token and does not resize the window.

`centers()` is the live window: `{ targets: [{ id, nx, ny, x, y, w, h, play }] }`. `nx` and `ny` are the center as fractions of `innerWidth` and `innerHeight`. `play` is true only for a lit stone. Hidden buttons are omitted. Back is omitted on the menu. Another is omitted when the ceiling is met. Eyes, the banner, the canvas, and the ring are omitted.

Menu chrome may sit 8 px in from the screen edge. Lit stones sit at least 16 px in. Page pad is 8 px. Narrow layout (the menu card uses the full width minus a small margin) starts under 480 px. Do not invent a fifth screen for a short window.

## Generator

The functions are pure. The same arguments return the same value. They do not read the clock, the DOM, the query string, the network, or `speechSynthesis`. They live in the page script. There is no shared file.

Ids are `"r,c"` with no spaces. Row 0 is the top. Column 0 is the left. An edge is one unordered pair of orthogonally adjacent open cells, stored once, the two ids in row-major order joined by `"|"`. Row-major order means the smaller `r * cols + c` comes first. Example on a 5-column grid: cells `4,2` and `3,2` store `"3,2|4,2"`.

Chebyshev distance is `max(|r1 - r2|, |c1 - c2|)`.

Compass order, used every time a neighbor list is built before a shuffle: north `(-1, 0)`, east `(0, 1)`, south `(1, 0)`, west `(0, -1)`.

Opposite means one step north and one step south, or one step east and one step west.

### Return value

`mazeGenerate(band, seed)` returns `{ band, rows, cols, cells, edges, start, goal, stones, solution, deadEnds, fallbackUsed }`.

- `cells` is the open floors, each `"r,c"`, sorted by row-major index.
- `edges` is the set described above.
- `start` and `goal` are cell ids.
- `stones` is the dwell points, a subset of `cells`.
- `solution` is the stone ids on the unique start-to-goal route, in walk order. This list is not `state().solution`. The page stores `solution.length - 1` on `state().solution`.
- `deadEnds` is `{ id, length }` for each dead-end leaf, in attachment order. The page stores `deadEnds.length` on `state().deadEnds`.
- `fallbackUsed` is false when a build passed, and true only when the fallback in this section was returned.

`mazeLit(maze, stoneId)` returns the stone ids that are legal dwells from that stone.

`mazeWalk(maze, fromId, toId)` returns the cell ids from `fromId` through the open corridor to `toId`, both ends included, or `null`.

`mazeSolution(maze)` returns the stone ids of the unique start-to-goal route. `solution.length - 1` is the promised length.

### RNG

`makeRng(seed)` takes a uint32. `rngUint(rng)` returns the next uint32. `>>>` is the unsigned shift. `imul(x, y)` is `Math.imul`.

1. `state = seed >>> 0`.
2. `state = (state + 0x6D2B79F5) >>> 0`.
3. `t = imul(state xor (state >>> 15), state or 1)`.
4. `t = t xor (t + imul(t xor (t >>> 7), t or 61))`.
5. Return `(t xor (t >>> 14)) >>> 0`.

`rngInt(rng, lo, hi)` is `lo + (rngUint(rng) % (hi - lo + 1))`, both ends included.

`rngShuffle(rng, items)` copies the array and runs Fisher-Yates from the last index down to 1: at index `i`, swap `i` with `rngInt(rng, 0, i)`. A list of length 0 or 1 makes no `rngInt` call. A list of length 2 makes one.

Seed 1 must yield these first four uints, in order: `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`.

`playSeed(clockMs, counter)` is `imul( ((clockMs >>> 0) xor imul(counter >>> 0, 0x9E3779B9)), 0x85EBCA6B ) >>> 0`. `playSeed(1, 1)` is `0x3B230FE8`.

Do not raise the try caps below to hide a different rng order. Seeds 1 and 2 accept inside these caps when this order is followed.

### What a stone is

On Tiny Tots every open cell is a stone.

On the other bands, a cell is a stone when it is the start, the goal, a dead-end leaf, a branch (three open neighbors), or a corner. A corner is a degree-2 cell whose two neighbors are not opposite. A straight degree-2 cell is not a stone. One dwell walks that whole straight run. A bend is a new dwell. Intermediate spur cells are straight degree-2 cells and are not stones. The spur leaf is a stone. The spur attachment is a branch and is a stone.

### Tiny build

`rng = makeRng(seed)`. `fallbackUsed` stays false, including when the walk fails and the straight row is used.

1. `dwells = rngInt(rng, 2, 4)`.
2. If `rngInt(rng, 0, 1)` is 0, lay a straight row `(0,0) … (0, dwells)` on a `1 × (dwells + 1)` grid. A dwell of 4 is five cells. Columns are `dwells + 1`. Rows are 1. Stop. This is a generated maze.
3. Otherwise grow a self-avoiding walk of `dwells + 1` cells on a `3×3`. The start is `(0,0)`, not a random cell. There is no straight-ahead spacer on this band. Each step builds the free orthogonal neighbors in compass order, `rngShuffle`s them, and takes the first. If the walk cannot step and it is still short, discard it and start again at `(0,0)`. The rng is not reset. Up to 20 attempts. If those 20 fail, lay the straight row from step 2 for this same `dwells`. That rescue is still `fallbackUsed: false`.
4. Edges join consecutive path cells. `rows` is 1 for the straight row and 3 for a `3×3` walk. `cols` matches. `cells` is the path only, not the unused `3×3` squares. Stones are every path cell. `start` is the first path cell. `goal` is the last. `deadEnds` is `[]`.

`mazeLit` on the goal returns `[]`. `mazeLit` on any earlier path cell returns a one-element list of the next path cell toward the goal. It does not return the cell behind.

### Older build

Bands and sizes, in order, each size with a fresh `makeRng(seed)`:

| Band | Sizes, stop at the first that accepts |
| --- | --- |
| `little` | `5×5` only |
| `shop` | `6×6`, then `8×8` |
| `rush` | `10×10`, then `12×12` |

Windows, both ends included:

| Band | `L` dwells | `D` dead ends | Spur length |
| --- | --- | --- | --- |
| `little` | 4 through 7 | 1 through 2 | 1 through 1 |
| `shop` | 8 through 14 | 3 through 6 | 1 through 3 |
| `rush` | 12 through 20 | 6 through 12 | 1 through 5 |

For one size, try up to 40 builds on that same rng. Accept the first build that passes every check in the build. If the size's 40 builds fail, restart `makeRng(seed)` on the next size. If every size fails, return the fallback and set `fallbackUsed`. Do not start a 41st build.

A failed build keeps every rng call it already made. The next build continues that stream. Do not call `makeRng` between builds of the same size.

**Turn-dwell count**, before spurs exist: path stones are the first cell, the last cell, and every corner. Turn-dwell count = that number minus 1. A straight interior is a path cell that is not the first, not the last, and not a corner. Count those. Recompute both numbers after every extension and after every deletion.

One build:

1. `L = rngInt` in the dwell window. `D = rngInt` in the dead-end count window. `goalD = L - D`. If `goalD < 1`, this build fails. Do not pick a start cell.
2. Start at a random cell: `rngInt(rng, 0, rows - 1)` for the row, then `rngInt(rng, 0, cols - 1)` for the column. `dir` is empty and `dirSteps` is 0. For up to 80 extensions, grow a self-avoiding orthogonal walk on the full rectangle.
   - Let `straight` be the in-bounds, unvisited cell one step along `dir`, or empty if there is no `dir`.
   - If `dir` exists, `dirSteps < 2`, and `straight` exists, the only option is `straight`. Do not call `rngShuffle` for that step. This spacer is what lets co-lit stones sit at Chebyshev distance 2.
   - Otherwise collect the free neighbors in compass order. If none, stop the walk. `rngShuffle` that list. If `straight` is in the shuffled list, move it to the front and leave the others in their shuffled order. Step to the first.
   - A step that keeps `dir` increases `dirSteps` by 1. A step that changes `dir` sets `dirSteps` to 1. The first step sets `dir` and sets `dirSteps` to 1.
   - After the step, if the turn-dwell count is at least `goalD` and the straight-interior count is at least `D`, stop the walk. That early stop is success, not failure.
3. While the turn-dwell count is above `goalD` and the path has more than 2 cells, delete the last cell and recompute. No rng. The loop is bounded by the path length.
4. Fail the build unless the turn-dwell count is exactly `goalD` and there are at least `D` straight interiors.
5. Attach `D` spurs. Shuffle the straight-interior cells once, in path order before the shuffle. Then, for each spur:
   - Draw `len = rngInt` in the band's spur-length window. Call it even when the window is 1 through 1, so Little Pals still consumes one uint.
   - Walk the shuffled interiors from the front. Skip a cell already used as an attachment.
   - From a candidate, take the two compass directions that are not along the path through that cell, in compass order, then `rngShuffle` those two. Never step back along the path.
   - The spur is a straight line of `len` free in-bounds cells. Free means not on the path and not on an earlier spur. Keep the first direction that fits. Mark that path cell used. Record the leaf and the length `len`.
   - A candidate that fits neither direction is left unused, so a later spur may try it. Each spur scans the list once.
   - Fail the build if any of the `D` spurs cannot be placed.
6. Solution stones are the start, the end, every corner, and every spur attachment, in path order. Fail unless `solution stones - 1 === L`.
7. Stone neighbors are the next solution stone along the path and, from an attachment, the spur leaf. Intermediate spur cells are not stones. Fail if any stone has more than 3 neighbors, or if any two neighbors of the same stone have Chebyshev distance under 2.
8. Rush: fail unless at least two spurs have length 3, 4, or 5.
9. Also fail this build if `mazeLit(start)` is not exactly one id. A legal start is an endpoint, so this passes when the walk is intact.

Open cells are the path plus the spur cells. Edges are each path step and each spur step, including the step off the attachment. `start` is the first path cell. `goal` is the last. `mazeLit` returns every stone neighbor, in this order: the previous solution stone if any, the next solution stone if any, then the spur leaf if this cell is an attachment. A leaf's only neighbor is its attachment. The start of a fresh maze lights one stone. A corner lights two. A spur attachment lights three: back, onward, and the spur. The check is `<= 3`, every neighbor lit, and no non-neighbor lit.

### Walk

`mazeWalk(from, to)` returns `null` unless `to` is a stone neighbor of `from`. Otherwise it returns the open cells in order: `from`, the straight interior cells, then `to`. Consecutive cells differ by exactly one row or exactly one column, not both, and that pair is an edge. The polyline does not contain a cell off the edge, so it does not cut a corner and does not cross a wall. A stone two forks away is not a neighbor, so the walk is `null`.

On the older bands, stone neighbors and `mazeLit` are the same set. On Tiny Tots, the cell behind is a stone neighbor in the graph, but `mazeLit` does not list it. `mazeWalk` to that behind cell still returns the two-cell polyline, because it is a neighbor. `move` rejects it anyway, because it is not lit, and returns `null` without moving the token.

The page `move` of an id that fails `mazeWalk`, or that `mazeLit` does not list, does not change the token.

### Fallbacks

Used only after the try caps. Each one passes the same structural checks. Coordinates are `r,c` in path order. Spur lists omit the attachment and follow the open steps. `fallbackUsed` is true on these objects. Tiny's straight-row rescue is not this fallback.

**Tiny.** Row `(0,0) (0,1) (0,2) (0,3)`. Rows 1, columns 4. Dwells 3. No spurs. Stones are all four cells. `mazeLit(0,0)` is `0,1`. `mazeLit(0,3)` is empty.

**Little, 5×5, dwells 6, one spur of length 1.** Path: `0,0 1,0 2,0 3,0 4,0 4,1 4,2 4,3 4,4 3,4 2,4 1,4 0,4 0,3 0,2 0,1 1,1`. Spur off `4,2`: `3,2`.

**Shop, 6×6, dwells 10, spur lengths 2, 3, 3, 2.** Path: `2,2 2,1 2,0 3,0 4,0 5,0 5,1 5,2 5,3 5,4 5,5 4,5 3,5 2,5 1,5 0,5 0,4 0,3 0,2 0,1 0,0 1,0`. Spurs:

- `4,1 4,2` off `4,0`
- `4,3 3,3 2,3` off `5,3`
- `1,4 2,4 3,4` off `0,4`
- `3,1 3,2` off `3,0`

The fourth spur is the straight east run off `3,0`. Attaching those same two cells to `2,1` bends the spur, makes `3,1` a corner, and puts stone neighbors at Chebyshev 1, so that reading fails the checks. Use `3,0`.

**Rush, 10×10, dwells 14, spur lengths 5, 3, 3, 4, 5, 1, 4, 5.** Path: `6,0 6,1 6,2 6,3 6,4 6,5 6,6 6,7 6,8 6,9 7,9 8,9 9,9 9,8 9,7 9,6 9,5 9,4 9,3 9,2 9,1 9,0 8,0 7,0 7,1 7,2 7,3 7,4 7,5 7,6 7,7 7,8 8,8`. Spurs:

- `5,6 4,6 3,6 2,6 1,6` off `6,6`
- `5,4 4,4 3,4` off `6,4`
- `5,1 4,1 3,1` off `6,1`
- `8,1 8,2 8,3 8,4` off `8,0`
- `5,8 4,8 3,8 2,8 1,8` off `6,8`
- `8,6` off `7,6`
- `5,7 4,7 3,7 2,7` off `6,7`
- `5,5 4,5 3,5 2,5 1,5` off `6,5`

The fourth spur is the straight east run off `8,0`. `8,1` also touches `7,1`, but a spur south from `7,1` is not the line `8,1 8,2 8,3 8,4`, and that attachment fails Chebyshev. There is no edge from `8,6` to `9,6`. The one-cell spur is only off `7,6`. A wall stays between `8,6` and the path on row 9, so the graph stays a tree.

### Generator checks

`engineSelfCheck` runs these. It does not build a Sudoku, a math item, a word search, or a flash card. Those games are other files.

For seeds 1 and 2, each band, `fallbackUsed` is false.

- `edges.length === cells.length - 1`, and every cell is reachable from `start`. Routes = 1.
- `mazeSolution` is the only start-to-goal stone path, and `solution.length - 1` is inside the band's dwell window.
- Dead-end count and lengths match the band table. Tiny has none.
- `mazeLit(start)` has one id on a fresh maze for every band. Every lit id is a stone neighbor. Every stone neighbor is lit, except Tiny, which lights only the successor toward the goal. Lit count is never above the band maximum (1, or 3).
- On Little, `mazeWalk` into the spur leaf returns a polyline that ends on that leaf. `mazeLit` of that leaf contains the branch behind it and does not contain `start` unless the branch is the start.
- For every legal `mazeWalk`, each step is an orthogonal edge. No step is diagonal. No step's pair is absent from `edges`.
- `mazeWalk` from `start` to a solution stone that is not the first step returns `null`. The token cannot cross a wall or skip a fork.
- Every pair of ids in one `mazeLit` result has Chebyshev distance at least 2. Tiny has only one lit id, so that pair check is empty.
- The four fallback mazes pass the same structural checks. `fallbackUsed` is ignored on that pass.

Loops stop at 20, 40, and 80. A failed search still returns.

## Self-check cases

`selfCheck` runs the family cases first, then the maze cases, then `engineSelfCheck`, and concatenates the failures. `ok` is true only when that list is empty. Each failure is one sentence that starts with the case id. Example shape: `tiny-walk: a 660 ms look left the token on the start.`

The check drives time only through `step`, from 10000. Assertions read `state()`, `centers()`, `targetsAt`, `dwell`, `eyes`, and the return value of `move`. They do not read screenshots and they do not wait on a frame. `engineSelfCheck` does not touch the DOM and does not call `Date.now`, `setTimeout`, `setInterval`, `requestAnimationFrame`, `fetch`, `WebSocket`, or `speechSynthesis`.

Spy on `localStorage.setItem` and `localStorage.removeItem` before the cases. Spy on `speechSynthesis.speak` if `speechSynthesis` exists. Restore both after the cases.

`family-no-storage` is asserted after every other case, including the engine cases. `family-query-inert` is asserted at the start, while the screen is still the menu, and the junk value is checked again at the end.

### Family

1. `family-guards`. At the start, `checking` is true, reconnect is suppressed, and the socket is null.
2. `family-no-storage`. After the later cases, `localStorage` has no key this game wrote, `pop-pals-v1` is unchanged from the start of the check, and the page never called `setItem` or `removeItem`.
3. `family-play-dwell`. `seed(1)`, then `start("tiny")`. Take the lit stone from `centers()`. `step(10000)`, `look` at that stone, `step(10000)` to enter. `step(10640)` does not change `token`. `step(10660)` sets `token` to that stone. Reset with `seed(1)` and `start("tiny")`. `tap` on that stone runs the same writer and sets the same `token`.
4. `family-menu-dwell`. Call `go("menu")` first if a previous case left the maze up. A look on Let's go: `step` at entry, `step` 600 ms later does not change the screen, `step` at 1300 ms from that same entry calls `start` and the screen is `maze`. From the maze, a look on Back: 600 ms leaves the screen on `maze`, 1300 ms from that entry calls `go("menu")`.
5. `family-pointer-blocks`. On a fresh Tiny maze, `hold` on the lit stone, then a 660 ms look, does not commit. `release`, then a fresh 660 ms look, does.
6. `family-stale`. On a fresh Tiny maze, enter the lit stone so the fill is above 0 and the token has not moved. Turn sample refresh off. `step` 500 ms past the last stamp. `dwell()` is 0 and `token` is unchanged. Turn refresh back on.
7. `family-target-size`. On the menu and, after `start("tiny")`, on the maze: `targetsAt(390, 844)` and `targetsAt(1024, 768)` both return `ok` true and a short side of at least 72 for every target. `targetsAt(300, 600)` returns `ok` false and no boxes. A box under 72 px fails the case. The call does not shrink a live button.
8. `family-eyes`. `eyes()` has only the boolean keys `on` and `live`. A fresh test look sets `live` true with the socket still closed. The Eyes chip is shown for that look. `releaseLook` hides it.
9. `family-mute`. The mute button's visible text is empty. It contains two inline speaker SVGs. The words Mute and Sound are not visible. `tap` sets `state().muted`. A second `tap` clears it.
10. `family-query-inert`. The harness opens the file with `selfcheck` plus one junk flag whose value looks like a URL or a tag. That value is not in the document text or in any attribute. At the start of the check the screen is `menu` and no socket was opened. The junk flag did not call `start`.
11. `family-plain-state`. `state()` can be turned into JSON and contains `level`, `screen`, `banner`, and `muted`.

### Maze

These run after the family cases. Pin `seed(1)` before each `start` unless the case says otherwise.

1. `tiny-one-stone`. `start("tiny")` lights exactly one stone. `deadEnds` is 0. `solution` is 2, 3, or 4. `routes` is 1. Repeat the numbers for `seed(2)`.
2. `tiny-walk`. A 640 ms look on that stone does not change `token`. A 660 ms look sets `token` to that stone and not to a later one.
3. `floor-ignores`. A 660 ms look on a point that is not a button leaves `token` where it is. `move("nope")` returns `null`, leaves `token`, and the string `nope` is not in the document.
4. `dead-end-return`. `start("little")`. Search from the token without taking the goal. From the current stone, each lit id other than the stone just left and other than the goal is a candidate, in row-then-column order. `move` to a candidate. If the only lit stone is the one just left, this is the spur: the screen is `maze`, `token` is not the start, and that stone behind is lit. `move` back to it and stop. If it is not a spur, search onward from there, then `move` back and try the next candidate. Bound the search by the open-cell count. Do this for seed 1 and seed 2. A Little maze that never finds a spur fails the case.
5. `one-route`. A pinned Shopkeeper maze, seed 1, has `routes` 1.
6. `lit-cap`. For each band, with seeds 1 and 2, the lit stones are neighbors of the token and the count is inside the band maximum: 1 on Tiny, at most 3 otherwise. A stone in `state().stones` with `lit` false is not a button in `centers()`. `move` of that id returns `null` and does not change `token`.
7. `separation`. Every pair of lit stones in `centers()` has at least 8 px of dead floor between the button squares, and each short side is at least 112. When `innerWidth >= 390` and `innerHeight >= 700`, every pair also has center distance at least twice the painted diameter. On Little seed 1, walk to a stone that lights two or three neighbors (do not enter the goal). `targetsAt(390, 844)` there uses diameter 112 and center distance at least 224. `targetsAt(1024, 768)` uses 160 only when every lit pair can keep center distance at least 320; otherwise 112. A fresh Tiny maze has one lit stone, so the pair gap is vacuous and the diameter is 160 when that disk fits. Never under 112.
8. `goal-rest`. From a fresh maze, walk to the goal one lit neighbor at a time. If several stones are lit, try a neighbor, and if the new lit set is only the stone just left, walk back and try another. Bound the steps by the number of open cells. Landing on the goal calls `go("rest")`. The banner does not say lose or fail. `state()` has no `msLeft`, best, lives, score, wrongs, or rank. The rest card has no grade.
9. `no-speech`. After the play above, `speechSynthesis.speak` has not been called. `state()` has no speech field.
10. `no-jump`. One successful `move` returns a polyline whose ends are the old token and the new token. Each step is an orthogonal edge. On Tiny the polyline has two cells. On the older bands the polyline's only stones are the two ends. `move` from the start to a solution stone that is not the first lit stone returns `null` and does not change `token`.

### Engine, after the maze cases

`engineSelfCheck` returns `{ ok, failures }`. Merge its failures. Call order:

1. `rngUint` four times on `makeRng(1)`. Compare to `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`. Compare `playSeed(1, 1)` to `0x3B230FE8`. `rngInt` on a fresh `makeRng(1)` stays inside `0..3`.
2. `mazeGenerate` for `tiny`, `little`, `shop`, and `rush`, seeds 1 and 2. Run the generator checks, including `mazeLit` and `mazeWalk` from every solution stone to each lit id and to one non-neighbor.
3. Run the same structural checks on the four fallback mazes.

## README paragraph

Paste this bullet into the Play list, after Saddle Up! and before any later game. Paste the section after the Saddle Up! section and before `## Add a game`.

```markdown
- [Maze](https://calledtoconstruct.github.io/games/maze/)
```

```markdown
## Maze

A maze for ages 2 and up. A blue meeple stands on the path. Look at the next dot and the meeple walks there. The eyes do not have to follow the corridor. A wrong turn bends, and you walk back out. It is not a lost game. Tiny Tots is a short path, and some deals add one wrong turn. Little Pals, Shopkeeper, and Rush light up to three dots. On those mazes one wrong path is longer than the way out, so the longest walk is not always the exit. There is no clock.

Open `maze/index.html` in a browser, or play [Maze](https://calledtoconstruct.github.io/games/maze/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at a lit stone for 650 ms, until the ring fills, and the token walks there. Walls, the floor, and a stone that is not lit do nothing. Menu buttons take twice that, 1300 ms, so reading one does not press it. Mouse and touch work in the same maze. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.
```

The README does not mention the check flag, the window hook, a grid size, or a score.

## Key Decisions

- **Clock.** Play dwell is 650 ms. Menu dwell is 1300 ms. The ring fills from 0 to 1 across that whole interval. 640 does not commit. 660 does. There is no 200 ms onset and no second timer. `LEAVE_MS` is declared as 280 and is not a leave grace. A look off the button cancels on the next `step`.
- **Trail.** It draws only while the fill on a lit stone is already above 0. Same clock as the ring. It does not commit and it is not a target. Reduced motion skips it and still walks.
- **Builder.** A newest-cell self-avoiding walk, a two-step straight spacer, then straight spurs. Not a full-rectangle growing tree, not Sidewinder, not binary tree, not a braid. Corners stay stones. Solution length is dwells after straight runs collapse.
- **Grids.** Little Pals is 5×5 only. Shopkeeper tries 6×6, then 8×8. Rush tries 10×10, then 12×12. Tiny Tots does not compress: every open cell is a stone, and only the next cell toward the goal is lit.
- **Little Pals lights three at a fork.** The age note's "two lit stones" would hide the way back, the way on, or the dead end. The cap is three. Every stone neighbor is lit.
- **Walk.** One dwell on a lit stone walks the open corridor to that stone, including the straight cells in between. `mazeWalk` to a non-neighbor returns `null`. `move` returns `null` and does not move the token. A dead end does not reset the maze.
- **`state().solution` is the dwell count.** The generator's `solution` is the id list. The hook's `solution` is `length - 1`, so Tiny reads 2, 3, or 4. `state().deadEnds` is the count, not the `{ id, length }` list.
- **Shop fallback attachment.** The cells `3,1 3,2` leave `3,0`. The reading "off `2,1`" fails Chebyshev. Rush's `8,1 8,2 8,3 8,4` leaves `8,0`, not `7,1`.
- **Lit buttons are not cell-pitch disks.** A 10×10 grid inside 390 px cannot put 112 px stones on the raw cell centers and still keep a 224 px gap. The canvas holds the corridors. The lit-stone buttons are placed with the gap rule. Unlit gold marks may be smaller than 112 px because they are not controls. Lit stones never go under 112. Neighbors are not hidden to buy a gap.
- **Another does not call resetting `start`.** `start` zeroes the sitting. Another deals the second maze and keeps `finished`, or the ceiling of 2 would not exist.
- **No speech, no timer, no lives, no storage.** `moves` is a hook field for how many walks committed. It is not painted and it is not a score.
- **This file's `engineSelfCheck` is RNG plus maze.** The other four puzzles are other pages. Putting them here would be a second game in the maze file.
- **Token updates inside the commit.** The check does not wait for the cosmetic slide.

## Out of scope

- A second file, a shared gaze file, a bundler, or an asset folder.
- Editing Pop Pals, Sugar Counter, Sun Garden, Saddle Up!, or the other four puzzle pages.
- `localStorage`, cookies, `fetch`, remote pictures, remote fonts, and remote audio.
- `speechSynthesis`, a spoken hint, or a buzzer.
- A round timer, a personal best, lives, a score, a streak, a rank, or a fail screen.
- Steering the corridor with the eyes, dragging a path, or dwelling the start and the goal as a pair.
- Blink, wink, or a gaze commit that is not a dwell on a button.
- A third dwell time, an onset timer, or a 280 ms leave grace that can finish a commit.
- Sidewinder, binary tree, braided loops, and a full-rectangle growing tree.
- A 4×4 Little Pals maze, a 7×7 Shopkeeper maze, or an 8×8 Rush maze.
- Hiding a lit neighbor so the stones fit.
- A Done button, a Harder flag, a Backward flag, or a fifth band.
- A typed name, a text field, or a parent mode beyond the presence flags `play` and `rush`.

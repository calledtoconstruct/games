# Five games, one page contract

**Author.** Architecture member, design committee.

**Date.** 2026-10-18.

**Status.** Accepted for the committee. Later design files may fill word lists, drawings, and banner sentences. They may not invent a second gaze client, a second dwell time, a second screen writer, or a second file of script.

This note locks the page shape for the five games researched on 2026-10-18: picture-and-word cards, missing-number questions, waypoint mazes, picture-and-digit Sudoku, and condensed word search. The level recipes come from those notes. The page contract comes from Saddle Up! and from the family rules already shipped in this repo.

Pop Pals, Sugar Counter, Sun Garden, and Saddle Up! are not edited. Pop Pals is not a template for storage.

## What all five share

The five games copy one behavior. They do not import it. There is no `gaze.js`, no shared folder, and no build. The copy source is the gaze client in `saddle-up/index.html`. Where that file and Sugar Counter disagree about startup order, follow Saddle Up!: read the query before `requestAnimationFrame` and before `connectGaze`. Where they disagree about the clock clamp, follow Sugar Counter, as Saddle Up! already does: a step advances a sitting clock only when the gap is greater than 0 and less than 120 seconds.

A review that diffs the five gaze blocks should find the same constant names, the same hello, the same framing, the same dwell choice, and the same pointer guard. The only intended difference is which button the finished dwell activates.

### Gaze hello, dwells, and the pointer

The socket URL is the literal `ws://127.0.0.1:7081`. It is not built from `location`, from a query value, or from a message. An `https` page does not open it and does not fall back to another host. Reconnect waits 2000 ms, and only if the check flag is absent.

On open, the page sends the nine hello bytes 1, 4, 0, 0, 0, 0, 5, 0, 0. Framing is a type byte, a little-endian unsigned 32-bit length, then the payload. Type 1 with at least 56 bytes is a sample. Type 4 with at least 72 bytes is the homography, nine little-endian floats. A non-empty type 4 shorter than 72 bytes is ignored. An empty type 4 clears the homography. A length above 1 MiB resets the buffer. Samples and the homography stay in memory. They are not logged, not put in `state()`, and not written into the page.

Constants, spelled this way in every file:

- `GAZE_URL` is `ws://127.0.0.1:7081`
- `PLAY_DWELL_MS` is 650
- `MENU_DWELL_MS` is 1300
- `GAZE_FRESH_MS` is 500
- `LEAVE_MS` is 280

There is no third dwell. There is no onset added on top of 650. The research notes for Sudoku and the maze asked for a quiet first 150 ms inside the play dwell. That quiet period is rejected here. It would be a second timer, and the five games would not paint the ring the same way. The ring fills across the whole dwell, as it does in Saddle Up!. A glance still fails because 650 ms is the commit, not because a hidden onset consumed part of it.

Every selectable thing is a real `button`. Gaze hit-testing is `elementFromPoint`, then the nearest `button`. A button with `data-play="true"` dwells 650 ms. Every other button dwells 1300 ms. The canvas is not a hit target. There is no geometric walk, no blink, no wink, and no drag that commits a gaze. Menu chrome, including band choices, Let's go, Back to Menu, Done, Another, subject chips, and group chips, does not carry `data-play`.

One pointer at a time. A second contact is ignored until the first ends. While a pointer is down, gaze dwell does not advance and the ring fill stays at 0. The ring may still sit on the look. A pointer press clears a gaze dwell that was in progress. Releasing the pointer does not itself fire the button. A tap and a finished look call the same writer. `look` only places the test point. `step` is what moves the dwell. `tap` is that writer with no wait.

The Eyes chip is the only gaze health light. It shows the word Eyes when the socket is open or a test look is held, and it is hidden otherwise. The ring is green, sits above the page, ignores pointer events, and is shown only while the sample is fresher than 500 ms. A stale look does not commit.

`look` during the check stamps freshness from the `step` clock, starting at 10000. `performance.now()` does not decide a dwell during the check. No `setTimeout` string, and no wait, is part of the check. Game clocks that are real ceilings (the flash-card sitting only) advance inside the 120 second clamp, so a test steps in chunks. The commit itself still happens on a single 660 ms step, because the dwell reads the timestamp, not the sitting clock.

### Page skeleton

Regions, not paint rules. One HTML document. No frame, no second script, no remote image, font, style sheet, or beacon. The head holds charset, a viewport meta, a cream theme color, and the game title. Nothing else is loaded.

The body, in this order:

1. An optional full-page canvas, `#view`, used only to draw. It never receives the gaze hit.
2. Chrome that is always in the tree: `#mute`, `#full`, `#back`, `#eyes`, `#gaze`, and `#banner`.
3. The menu card, `#screen-menu`.
4. The play host, `#play`, which holds the game buttons for every screen that is not the menu or the rest card.
5. The rest card, `#screen-rest`.
6. The self-check node is not in the authored markup. The loader creates it only when the flag is present.

`#mute` is a 72 px button with two inline speaker SVGs, one hidden at a time. It has no visible text Mute or Sound. The accessible name may say "Mute sound" or "Turn sound on". `#full` is a 72 px Fullscreen button. `#back` says Back to Menu, is a menu dwell, and is hidden on the menu. `#banner` is a status line, one short sentence, and it does not receive clicks. It is not a score, a timer, or a rank.

The menu card holds the game title, one line about what to do, the line Ages 2 and up, and four band buttons. Their ids are `tiny`, `little`, `shop`, and `rush`. A sun-gold Let's go button starts the highlighted band. Band buttons and Let's go are menu dwells.

**Menu names revised 2026-10-19.** The ids stay. The words on the buttons are chosen for that game, so a maze does not say Shopkeeper. The age line on each button stays. Do not copy one game's names onto another.

The rest card holds one calm sentence, an Another button, and uses the same Back control. It does not show a grade, a count of misses, a time, or a rank.

Visual language, named so the five files do not drift: cream cards, sun-gold primary buttons, ink on cream, the font stack Arial Rounded MT Bold, then Trebuchet MS, then Segoe UI, and the green gaze ring. Every live target has a short side of at least 72 px at a 390 by 844 pretend viewport and at 1024 by 768. A pretend viewport under 320 px wide fails the size case. The game does not pretend the targets fit by shrinking them.

**Revised 2026-10-19.** The sentence above is the 2026-10-18 rule. It is superseded where a 72 px floor would force the page to scroll. Eye tracking cannot use a target that is off the screen. The scene scales so every live control ends inside the viewport. Word search is the first page on this rule. Its `fit` flag is true when the boxes are inside, including at 300 by 600, and it shows the full grid instead of paging columns.

Sound, except speech in Flash Cards, is an oscillator started after the first pointer press or the first committed gaze action. Master gain is 0.5 unmuted and 0 muted, matching Saddle Up!, not Pop Pals and not Sugar Counter. Mute does not delete the buttons. `prefers-reduced-motion: reduce` skips decorative motion and still applies the commit. No recorded audio files.

### One hook shape

Each page assigns its `window` object as soon as the script runs, before the query is handled. The shared functions mean the same thing on every object. A test or a parent calls these. There is no second parent mode.

- `start(band)` sets the band when `band` is one of the four ids. An unknown band does nothing. It resets the sitting, then calls `go` to that game's first screen after the menu. It does not open the socket.
- `go(screen)` is the only writer of the screen id. Unknown ids do nothing. Other helpers may call `go`. They may not assign the screen themselves.
- `look(nx, ny)` places a test gaze at window fractions. It does not commit.
- `releaseLook()` drops the test gaze.
- `hold(nx, ny)` is a pointer down. A second hold is ignored.
- `release()` is the matching pointer up. It does not fire a button.
- `tap(nx, ny)` runs the writer for the button at that point, with no dwell, and does not leave the pointer down.
- `step(now)` is the only clock.
- `dwell()` returns the ring fill from 0 to 1.
- `eyes()` returns `{ on, live }` and nothing else. No coordinates.
- `centers()` returns window fractions and pixel boxes for every live target.
- `targetsAt(w, h)` returns those boxes for a pretend viewport.
- `state()` returns plain data. It always includes `level`, `screen`, `banner`, and `muted`. It never includes a DOM node, a gaze coordinate, the socket URL, or the query string.
- `seed(n)` pins the next generator or deck. It does not change a sitting that has already started. It is not read from the query.
- `selfCheck()` returns `{ ok, failures }`. Each failure is one sentence. The hook does not need a tracker.

`start`, a band button, and Let's go share one band writer. A play button and `tap` and a committed `look` share that button's writer. `render` runs before any of these return, including while the check flag is on and the frame loop is off.

### Screens

Every game uses `menu` and `rest`. The play screens are named on that game below. `go` shows one screen and hides the others. Back always calls `go("menu")`. Reaching a cap or a goal calls `go("rest")`. Another calls `start` again for the same band, which calls `go`. None of those paths write storage.

Deep links are presence flags for the person building the page. They are not a parent mode. They run only when the self-check flag is absent. Each known flag calls `start` or sets one boolean. The value of a flag is ignored. Unknown flags do nothing. No flag changes the socket URL.

Shared flags:

- `play` calls `start("tiny")`.
- `rush` calls `start("rush")`.

### Self-check runner

`?selfcheck` is handled before any frame and before any socket. The page sets `checking` and `suppressReconnect`, calls `selfCheck`, creates a `pre` with id `selfcheck-result`, sets its text with `textContent` to the JSON result, and sets `document.title` to `SELFCHECK_OK` or `SELFCHECK_FAIL`. It then stops. It does not call `requestAnimationFrame`. It does not construct a WebSocket. It does not apply deep links. If a socket already existed, the check closes it and keeps reconnect suppressed.

The check drives time only through `step`, from 10000. Each case bounds its own steps. Assertions read `state()`, `centers()`, `targetsAt`, `dwell`, and `eyes`. They do not read screenshots and they do not wait on a frame.

### Family cases

Every `selfCheck` runs these first, under these names. The game cases below are in addition.

1. `family-guards`. At the start of the check, `checking` is true, reconnect is suppressed, and the socket is null.
2. `family-no-storage`. After the later cases have run, `localStorage` has no key for this game, `pop-pals-v1` is unchanged from the start of the check, and the page never called `setItem` or `removeItem`.
3. `family-play-dwell`. A look on a play target for 640 ms does not commit. A look for 660 ms does. A `tap` on that target runs the same writer.
4. `family-menu-dwell`. A look on Let's go or Back for 600 ms does not change the screen. A look for 1300 ms does.
5. `family-pointer-blocks`. `hold` on a play target, then a 660 ms look, does not commit. `release`, then a fresh 660 ms look, does.
6. `family-stale`. A look that `step` ages past 500 ms leaves `dwell()` at 0 and does not commit.
7. `family-target-size`. `targetsAt(390, 844)` and `targetsAt(1024, 768)` both report a short side of at least 72 for every target. `targetsAt(300, 600)` fails this case instead of shrinking a target.
8. `family-eyes`. `eyes()` has only the boolean keys `on` and `live`. A fresh test look sets `live` true with the socket still closed. The Eyes chip is shown for that look.
9. `family-mute`. The mute button's visible text is empty, it contains two inline speaker SVGs, and the words Mute and Sound are not visible. `tap` sets `state().muted`. A second `tap` clears it.
10. `family-query-inert`. With a junk flag whose value looks like a URL or a tag, that value is not in the document text or in any attribute, the screen is still the menu, and no socket was opened.
11. `family-plain-state`. `state()` can be turned into JSON and contains `level`, `screen`, `banner`, and `muted`.

The harness opens the file with `?selfcheck` plus one junk flag. The game cases must still pass on that URL.

### Files, license, storage

Each game adds one folder and one `index.html`, and adds one short section to `/home/joseph/code/games/README.md`. The Play list gains one link in the same list. The section sits after Saddle Up! and before Add a game. It has the same voice as the Saddle Up! section: what the child does, what each band adds, how to open the folder, then the gaze paragraph. The gaze paragraph names this game's targets, keeps 650 ms play and twice that for menu buttons, names `ws://127.0.0.1:7081`, the Eyes mark, the screen correction, mouse and touch, fullscreen on the calibrated panel, and the fact that an https page will not open the socket. It does not mention the check flag, the window hook, or a score.

The license stays the repo MIT license. Copyright 2026 Joseph Woolley. No game file adds a second license or a different copyright.

Designs, when they are written, live under `docs/superpowers/specs/` as one markdown file per game. They are not a script, and they are not created by this note. No game has a second JavaScript file, a module, or an asset folder.

New games do not write `localStorage`. They do not read it except for the storage case above. They do not set a cookie, send a beacon, or `fetch` anything.

### Threats on every game

A security review looks for these on each file. Game sections add the surfaces that are extra.

- The WebSocket host is pinned to `127.0.0.1` port 7081. The URL is a literal. Nothing concatenates a host, a query, or a payload onto it. There is no remote fallback and no proxy.
- No `innerHTML`, `insertAdjacentHTML`, or `document.write` of external data. External data means the query string, the socket payload, and the homography. Labels and the banner are authored strings assigned with `textContent`. Gaze bytes are numbers.
- No `eval`, no `new Function`, and no timer that takes a string.
- Speech text, where speech exists at all, is one fixed word from that game's authored list. The other games do not call `speechSynthesis`.
- No query-string injection into HTML. Flags are presence checks against a fixed list. The raw search string is never written into text or an attribute.
- `state()` and `eyes()` do not expose gaze coordinates. The check does not open the socket. There is no text field a child can type into.

Generator loops are bounded so a check cannot hang. Word search stops at about 20,000 placement attempts and then uses the stored fallback. Sudoku counts solutions by stopping at two. The check uses a pinned seed and does not treat wall-clock time as a pass or a fail.

### Rejected for all five

A shared gaze file. A bundler. One page that holds all five games. Blink or wink. A drag as the gaze commit. A 0–9 keypad. A countdown that can fail the child. A mistake tally, a life, a buzzer, or a rank. `localStorage` progress. Remote pictures, fonts, scripts, or symbol sets. A typed name. Editing Pop Pals. A third dwell time. A per-game rewrite of the hello or of the pointer rule.

## Flash Cards

**Folder.** `flash-cards`

**Title.** Flash Cards

**Window.** `window.flashCards`

**Why this shape.** The research splits hearing a word from reading one, keeps two to four large cards, and refuses preview speech. The family rule is narrower than that note: `speechSynthesis` is allowed only in this game, and only after a finished dwell on a word. Speech when a trial appears is rejected. Speech of a word the child did not dwell is rejected.

**Files.** `flash-cards/index.html` and the Flash Cards section of `README.md`. No Mulberry file, no credits file, and no SVG asset. Drawings are canvas, keyed by the authored word id. A word with no drawing is a failed check, not a network fetch.

**Screens.** `menu`, `subject`, `trial`, `rest`. `go` is the only writer.

`start("tiny")` lands on `subject`. Subject buttons are the subjects that band may use, and they are menu dwells. Choosing one calls `subject(id)`, which deals the sitting and calls `go("trial")`. The trial cap or the hidden minute ceiling calls `go("rest")`. Stop is a menu button that calls `go("rest")` early. Rest has no grade.

**Functions beyond the shared hook.**

- `subject(id)` deals that subject when the id is in the band's authored list. An unknown id does nothing and is not written into the page.
- `choose(id)` is the only writer of the current answer. The picture buttons and the printed-word buttons call it. An id that is not on the current cards does nothing.
- `hear()` is the prompt button's writer. It speaks that prompt word and does not choose a card.

`state()` adds `subject`, `phase` (`ask` or `show`), `direction` (`hear-picture`, `picture-word`, or `word-picture`), `prompt`, `choices`, `trial`, `cap`, `said`, and `lastChoice`. It does not add a wrong count, a score, or a lives field. `said` is the last word actually passed to speech, or null.

**Play, by band.** One subject per sitting. Each target is scheduled twice before a new word is added. Foils come from that sitting. Tiny Tots foils look unlike the target. From Shopkeeper up, foils may be neighbors.

- Tiny Tots. Two picture choices. Direction is only `hear-picture`. No printed choice. Deck of 6. Cap 8 trials, or 4 minutes of `step`, whichever comes first. Subjects: animals (dog, cat, bird, fish, cow, duck), food (milk, apple, banana, bread, cookie, water), and body (eye, nose, mouth, hand, foot, tummy). No colors, places, or time words.
- Little Pals. Three choices. Same direction, except at most one trial in four may be picture then printed word, and only for a word already heard this sitting. After two missed print trials, print stops. Deck of 8, or 6 for colors. Cap 12 trials, or 8 minutes. Adds clothing (hat, shoe, sock, coat, shirt, pants), toys (ball, car, doll, block, book, train), and vehicles (car, bus, bike, boat, plane, train). Colors are a spoken picture subject only: red, yellow, blue, green, black, white.
- Shopkeeper. Four choices. Default is picture, then printed word. About one trial in three is the reverse, pinned by `seed`. Deck of 10. Cap 16 trials, or 12 minutes. Adds household words and the less obvious animals from the research list. Color names may be printed words here.
- Rush. Four choices. About half the trials are reversed, pinned by `seed`. Deck of 12. Cap 16 trials, or 15 minutes. Adds the later subjects from the research note, including a short time set. No six-card layout.

The minute ceilings are hidden. There is no clock on the page. Caps are ceilings. Stop can end the sitting sooner.

**Speech.** This is the only game that may call `speechSynthesis`. The text is exactly one string from the authored list, chosen by id. It is not a sentence, not a query value, and not a banner. One utterance at a time. A new commit cancels the previous. Leaving the target before 650 ms cancels the pending commit and does not speak.

- A picture choice never speaks.
- A printed word speaks that word at the moment its 650 ms dwell commits, and not before.
- The prompt speaks only when its own dwell commits, through `hear`. That dwell does not select a card. Menus do not speak.
- A correct `choose` sets `phase` to `show` and shows the picture and the word together. It does not speak a second time if that word was already spoken for this commit.
- A wrong `choose` does not add a tally and does not auto-speak the answer. `phase` becomes `show` of the correct pair. The banner does not say fail or wrong. If the child dwells the shown correct word, that dwell speaks it. If they do not, the beat still ends. `step` across the show beat calls the next trial, or `go("rest")` at the cap.
- The card just missed ignores gaze until the look moves off it or `releaseLook` runs. The gaze client is unchanged. The writer drops the repeat.

**README.** The section says the child hears a word by looking at it, then looks at the matching picture, with more print on the older bands. It names the four bands in one or two sentences each. It says speech is the browser's own voice, after a finished look at a word, and that a sweep across the cards stays quiet. It includes the shared gaze paragraph, with the cards as the play targets and the subject buttons as the slow ones.

**Self-check, after the family cases.**

1. `tiny-two-pictures`. `start("tiny")` and `subject("animals")` set `choices` length 2, `direction` `hear-picture`, and a prompt from that list of six. No choice is a printed word. No `+`, `−`, or `=` is on the trial.
2. `speech-on-dwell`. A 640 ms look on the prompt leaves `said` null. A 660 ms look sets `said` to that exact authored word. A look that leaves at 640 does not speak.
3. `picture-silent`. `choose` of a picture does not change `said`.
4. `printed-word-speaks`. On a Shopkeeper print trial, a committed printed choice sets `said` to that choice and to no other card's word.
5. `miss-no-tally`. A wrong `choose` leaves no score, wrongs, or lives field. The banner does not contain fail or wrong. `phase` is `show` of the target, not of the foil.
6. `miss-cooldown`. A second gaze commit on the foil, without `releaseLook` or a move off it, does not speak again and does not change `lastChoice`.
7. `show-then-next`. `step` across the show beat increases `trial`, or sets `screen` to `rest` at the cap.
8. `repeat-before-new`. With a pinned seed, each target appears twice before a new word is introduced.
9. `little-print-budget`. Print trials are at most one in four, only for a word already heard. After two missed print trials, `direction` stays `hear-picture`.
10. `shop-four`. Four choices, and the default direction is `picture-word`.
11. `rush-half`. With a pinned seed, the reverse count is half of the cap, give or take the pinned schedule the spec will freeze. It is not all one direction.
12. `colors-not-print`. The Little Pals colors subject has six picture choices in the deck and no printed color word. A Shopkeeper color trial may print the word.
13. `cap-rest`. The trial cap calls `go("rest")`. The rest card has no grade. `step` of the Tiny Tots ceiling, in chunks under 120 seconds, also calls `go("rest")` with no fail banner.
14. `subject-reject`. `subject("not-a-subject")` does not change the screen and does not insert that string into the page.
15. `no-remote-art`. The document has no image URL, no font URL, and no script URL.

**Threats, beyond the shared list.** Speech is the sensitive call. The review checks that the utterance string is a member of the authored list, that picture ids cannot be spoken, that a query value cannot become the utterance, and that cancelling a short look cancels the utterance. `subject` ids pass through the authored table or they do nothing.

## Math Practice

**Folder.** `math-practice`

**Title.** Math Practice

**Window.** `window.mathPractice`

**Why this shape.** The research keeps one generator: a quantity, or three terms and one blank, answered on two to four large cards. Bands change the range, the operation, and which slot is blank. They do not add a keypad. This game never speaks.

**Files.** `math-practice/index.html` and the Math Practice section of `README.md`.

**Screens.** `menu`, `ask`, `rest`. `go` is the only writer. `start` lands on `ask`. Done is a menu button on `ask` that calls `go("rest")`. The cap does the same. Rest has no grade.

**Functions beyond the shared hook.**

- `answer(value)` is the only writer of the blank. The value must be one of the cards currently offered. A correct value fills the blank and sets `phase` to `show`. A wrong value leaves the problem unchanged.
- `next()` leaves `show` for the next problem, or calls `go("rest")` at the cap. `step` across the short show beat calls `next`.

`state()` adds `phase` (`ask` or `show`), `kind` (`quantity` or `equation`), `op` (null, `+`, `−`, `×`, or `÷`), the visible terms, `blank` (`set`, `more`, `result`, `part`, or `start`), `cards`, `filled`, `index`, and `cap`. No wrongs, streak, clock, or speech field.

**Play, by band.** Cards are the only answers. There is no pad, no drag, and no handwriting. A wrong card leaves the problem as it was. The banner does not say fail. That card ignores gaze until the look moves off it or `releaseLook` runs. The writer enforces that. The gaze client does not grow a cooldown timer.

Distractors are one neighbor off by one, plus the sum of the two visible numbers when the blank is a part, plus other legal values in range. No negatives. No numbers the band does not use.

- Tiny Tots. Pictures only. No plus, minus, or equals. Numbers 1–3. The blank is the matching set, or the set that is one more. Two cards. Cap 4.
- Little Pals. Dots beside digits. Totals stay inside 0–5. Result unknown first (`a + b = □`, then `a − b = □`). The second addend may be blank only after a result-unknown success in this sitting. The first slot is never blank. Three cards. Cap 6. The total is not on the left.
- Shopkeeper. Digits. Plus and minus only. Within 10, then within 20. Blank order is result, then either part, then the start. One of those families at a time. The total on the left waits until start-unknown, not the first problems. An optional last problem may be a repeated addend such as `2 + 2 + 2 = □`, still not multiplication. Four cards. Cap 8.
- Rush. Plus, minus, times, and divide, still three terms and one blank. Addition and subtraction stay within 20. Factors are one digit. The early times problems use 1, 2, 5, and 10, then 3 and 4, then 6–9, pinned by `seed`. Division is an unknown factor, not a new layout. Four cards. Cap 10.

Compare problems, two-step problems, parentheses, and remainders are out. Caps are ceilings. Done may end the sitting on the first problem.

**README.** The section says the child looks at the picture or the equation, then looks at the card that fills the hole. It names the four bands by range, not by a curriculum code. It says a miss stays on the page with no score and no clock. The gaze paragraph names the answer cards as the play targets and Done and Back as the slow ones. It does not mention speech.

**Self-check, after the family cases.**

1. `tiny-no-symbols`. The ask screen has two cards, values in 1–3, `kind` `quantity`, and no plus, minus, or equals.
2. `tiny-cap`. The fourth correct answer calls `go("rest")`. A further `answer` does nothing.
3. `little-result-first`. The first problem has `blank` `result`. `blank` `part` does not appear before one result has been filled. `blank` is never `start`. Every total is at most 5. There are three cards.
4. `little-no-left-total`. The equation is never shown as a total, then equals, then a blank expression.
5. `shop-order`. A pinned seed uses result, then part, then start, and does not mix those families inside one stretch of problems. `op` is never times or divide. No card is above 20.
6. `rush-factors`. The first times problems use only the factors 1, 2, 5, and 10.
7. `distractor-sum`. When `blank` is `part`, one card is the sum of the two visible numbers, the correct value is also a card, and every card is in range.
8. `wrong-unchanged`. A wrong `answer` leaves the terms and an empty blank. There is no wrongs field. The banner does not contain fail.
9. `cooldown-gaze`. A second look on that card, without leaving it, does not commit again. `releaseLook`, then a new look, can.
10. `done-slow`. A 600 ms look on Done does not change the screen. A 1300 ms look calls `go("rest")`. Rest has no grade.
11. `no-pad`. `targetsAt` does not list a ten-key pad. Card count is 2, 3, or 4.
12. `no-speech`. Nothing in the sitting calls `speechSynthesis`.
13. `pointer-shares`. `tap` on the correct card fills the same blank a 660 ms look fills.

**Threats, beyond the shared list.** Card labels are digits or picture ids from the generator's own range, assigned with `textContent`. A query value is not a term and not a card. This file does not reference `speechSynthesis`. The review checks that a miss cannot be turned into a stored tally.

## Maze

**Folder.** `maze`

**Title.** Maze

**Window.** `window.maze`

**Why this shape.** The research rejects steering a corridor with the eyes. The child dwells the next junction. The token walks the open floor. A look at a wall, the floor, or a later junction does nothing. The maze never resets. There is one solution. Tiny Tots is a single path, not a forked maze.

**Files.** `maze/index.html` and the Maze section of `README.md`. No stored picture of a maze. The grid is generated in the page.

**Screens.** `menu`, `maze`, `rest`. `go` is the only writer. `start` lands on `maze`. The goal calls `go("rest")`. Another calls `start` for the same band. There is no fail screen.

**Functions beyond the shared hook.**

- `move(stoneId)` is the only writer of the token's junction. A lit stone's button calls it. An unknown id, an unlit stone, or a stone with no open edge from the token does nothing.

`state()` adds `rows`, `cols`, `stones` (id, lit or not, and whether it is the goal), `token`, `moves`, `deadEnds`, `solution`, and `routes`. `routes` is 1 on every band except that Tiny Tots may report 1 as well, because a single path is one route. There is no `msLeft`, no best time, and no lives field. A personal-best pace is rejected. It would want storage, and the research left it off.

**Play, by band.** Generation is a seeded growing-tree perfect maze on a small orthogonal grid, then a compression: degree-2 cells become the inside of a corridor and are not stones. Dwell points are the token's neighbors in that graph, and the goal when the goal is one of those neighbors. The page regenerates until the row below is met and every pair of lit stones is separated by at least one stone's short side of floor. Stones stay inset from the screen edge. Sidewinder, the binary-tree algorithm, and braided loops are rejected. The check asserts structure, not the function name: one route, dead-end counts, and which stones are lit.

- Tiny Tots. One path on a 3×3, or three to four cells in a row. Not a full maze. Solution length 2–4 dwells. No dead ends. One lit stone, the next one only.
- Little Pals. 4×4 to 5×5. Solution 4–7 dwells. One or two dead ends, each one cell. Up to two lit stones.
- Shopkeeper. 6×6 to 8×8. Solution 8–14. Three to six dead ends, each one to three cells. Up to three lit stones.
- Rush. 8×8 to 12×12. Solution 12–20. Six to twelve dead ends, a few of them three to five cells. Up to three lit stones. No timer.

A finished dwell walks one open edge. `state().token` updates inside that commit, not at the end of the cosmetic walk, so the check does not need a frame. The token does not jump two forks in one `move`. A dead end is a legal arrival. The stone behind then lights. Looking through a wall does nothing. The floor is not a button. Stones are buttons with `data-play="true"`.

**README.** The section says the child looks at the next stone and the token walks the corridor. It says a wrong turn is a place you walk back from, not a lost game. It names the bands by how many choices light up, not by a pencil-maze grid size. The gaze paragraph names the stones as the play targets. It says the walls and the floor do nothing, and that menu buttons take twice as long.

**Self-check, after the family cases.**

1. `tiny-one-stone`. Exactly one stone is lit, `deadEnds` is 0, and `solution` is 2, 3, or 4.
2. `tiny-walk`. A 640 ms look on that stone does not change `token`. A 660 ms look sets `token` to that stone and not to a later one.
3. `floor-ignores`. A 660 ms look on a point that is not a button leaves `token` where it is.
4. `dead-end-return`. On Little Pals, `move` into the spur stays on `maze`, does not send the token to the start, and then lights the stone behind.
5. `one-route`. A pinned Shopkeeper maze has `routes` 1.
6. `lit-cap`. Lit stones are neighbors of the token, and the count is inside the band's maximum. A far junction is not lit.
7. `separation`. Every pair of lit stones is at least one stone-width apart in the boxes from `centers()`.
8. `goal-rest`. Moving onto the goal calls `go("rest")`. The banner does not say lose or fail. `state()` has no `msLeft` and no best.
9. `no-speech`. Nothing calls `speechSynthesis`.
10. `no-jump`. One `move` changes the junction by a single open edge. The cell count of that edge may be more than one. The fork count is one.

**Threats, beyond the shared list.** Stone ids are from the generated graph, not from the query. `move` of a string that is not a current stone id does nothing and is not inserted into the page. This file does not reference `speechSynthesis`. The review checks that a failed generation regenerates or keeps the last good maze, and does not draw a maze with two routes or with overlapping stones.

## Sudoku

**Folder.** `sudoku`

**Title.** Sudoku

**Window.** `window.sudoku`

**Why this shape.** The research treats a 9×9 of small cells as the wrong gaze target. The act is two looks: latch an empty cell, then dwell one large symbol. The first look does not write. Givens are many, and a singles-only solver must finish the board. There is no clock and no mistake tally.

**Files.** `sudoku/index.html` and the Sudoku section of `README.md`. Symbols are canvas drawings or text on buttons. No downloaded puzzle.

**Screens.** `menu`, `board`, `rest`. `go` is the only writer. `start` generates one board and lands on `board`. A finished board calls `go("rest")`. Another is a menu dwell that calls `start` again. It does nothing until the board matches the solution.

Zoom is not a screen. On a viewport where a 9×9 cell would be under 72 px, Rush does not show that grid. It stays on 6×6, or it sets `zoom` to a box id and enlarges that box. `zoom(boxId)` is the only writer of `zoom`. It is not `go`.

**Functions beyond the shared hook.**

- `latch(cellId)` latches an empty cell. A given cell does nothing. A second latch replaces the first. A latch does not write a symbol.
- `write(symbol)` writes into the latched cell when that symbol is live. A dead symbol, an unknown symbol, or a missing latch does nothing.
- `undo()` removes the last symbol the child placed. It does not clear a given. It does not drag.
- `hint()` lights one cell that is a single right now and marks the matching tray symbol. It does not write.
- `zoom(boxId)` sets or clears the enlarged box. An unknown id does nothing.

`state()` adds `size` (4, 6, or 9), `givens`, `empties`, `latched`, `tray` (each symbol and whether it is live), `placed`, `solved`, `hint`, `singles`, `solutions`, and `zoom`. `solutions` is 1. `singles` is true. There is no mistakes field, no rank, and no clock.

**Play, by band.** The tray is always on the board: one large button per symbol. Mouse, touch, and gaze use the same cells and the same tray.

- Tiny Tots. 4×4 pictures, 2×2 boxes, 12 of 16 given. Each empty cell is a naked single before any write. The tray offers only the symbol that fits.
- Little Pals. 4×4 pictures, 8–10 given. A short chain of naked singles. Hidden singles are not required. Illegal symbols are visible, dim, and dead.
- Shopkeeper. 6×6 digits, six 2×3 boxes, 24–28 given. Naked singles and hidden singles only.
- Rush. 6×6 digits with 18–22 given. A 9×9 is allowed only when every cell and every tray symbol keeps a short side of at least 72 px, with at least 40 givens, at least eight distinct digits, and a singles-only finish. Otherwise the page uses the 6×6, or box then cell. No 17-clue board.

The generator fills a complete grid, then clears cells only while two checks still pass: a search that stops at two solutions still finds one, and a singles-only solver still finishes. Tiny Tots adds the third check that every remaining empty cell is a naked single on the unsolved board. It stops at the given floor. It does not aim for a minimal puzzle. Givens on a 4×4 include at least three distinct symbols, on a 6×6 at least five, and on a 9×9 at least eight. Empties are spread across the boxes.

A wrong or dead look does nothing. There is no buzzer and no red tally. Undo and hint are play targets. Back, Another, and a new board are menu targets.

**README.** The section says the child looks at an empty square until it holds, then looks at the symbol that belongs there. It says the first look does not fill the square. It names the bands by grid and by how full the board starts, and it says nothing is timed and nothing counts mistakes. The gaze paragraph names empty cells, tray symbols, Undo, and Hint as the play targets.

**Self-check, after the family cases.**

1. `unique`. A pinned board on each band has `solutions` 1.
2. `singles-only`. `singles` is true, and playing only `hint` plus the pulsed symbol finishes the board.
3. `tiny-forced`. On the opening Tiny Tots board every empty cell is a naked single, and the live tray count is 1.
4. `little-range`. Givens are 8, 9, or 10 on a 4×4, and a hidden single is not required to finish.
5. `shop-boxes`. The size is 6, givens are from 24 through 28, and both naked and hidden singles are allowed. A guess is not required.
6. `symbol-bound`. A 4×4 shows at least three symbols, a 6×6 at least five, a 9×9 at least eight.
7. `latch-not-fill`. A 660 ms look on an empty cell sets `latched` and does not change `placed`. A 640 ms look does not latch. A 660 ms look on the live symbol then writes.
8. `given-ignores`. A 660 ms look on a given cell leaves `latched` null.
9. `dead-symbol`. `write` of a dead symbol does not change the cell and does not create a mistakes field.
10. `undo-child-only`. `undo` removes the last placed symbol and does not change a given. A second `undo` at the start of the board does nothing.
11. `hint-does-not-write`. `hint` sets `hint` and does not change `placed`.
12. `another-gated`. Another before `solved` does not change the board. After the board matches, Another calls `start` and deals a new grid.
13. `rush-narrow`. `targetsAt(390, 844)` does not report a 9×9 of cells under 72 px. Rush on that viewport is a 6×6, or a zoomed box whose cells are at least 72.
14. `no-pencil`. No cell contains a stack of candidate buttons.
15. `no-tally`. `state()` has no mistakes, rank, or clock. A dead write leaves the banner without fail.
16. `no-speech`. Nothing calls `speechSynthesis`.

**Threats, beyond the shared list.** Cell ids and symbols come from the fixed alphabet for that size. `write` of any other string does nothing and is not inserted into the page. The homography cannot become a symbol. This file does not reference `speechSynthesis`. The review checks that the generator cannot emit a board with two solutions, and that the 9×9 path is unreachable when the cells would be under 72 px.

## Word Search

**Folder.** `word-search`

**Title.** Word Search

**Window.** `window.wordSearch`

**Why this shape.** The research marks a word by the first cell and the last cell, not by a drag. Gaze, mouse, and touch share that two-step act. A tap is a commit with no wait. A look commits at 650 ms. The bank is not how a word is scored. This game does not speak. The research asked for a bank dwell to say the word. That call is rejected. A bank dwell shows the word, and on the younger bands shows its picture. It does not mark the word found.

**Files.** `word-search/index.html` and the Word Search section of `README.md`. Groups, including one fallback layout each, are data in that file. No second word list file.

**Screens.** `menu`, `group`, `puzzle`, `rest`. `go` is the only writer. `start` lands on `group`. The groups on that screen are the ones whose home band is the current band. Choosing one calls `group(id)`, which places the grid and calls `go("puzzle")`. The last word calls `go("rest")`. Another returns to `group` through `go("group")`, not into a new random band.

**Functions beyond the shared hook.**

- `group(id)` places that group when the id is authored for this band. An unknown id does nothing. Play uses a fresh seed. The check calls `seed` first.
- `anchor(cellId)` is the first-cell writer. A 650 ms dwell or a tap on a cell calls it. A second commit on the same cell clears the anchor.
- `commit(cellId)` is the second-cell writer. If the straight segment between the anchor and this cell, in either order, is exactly one unfound answer in an allowed direction, that word locks and its bank chip is marked. If it is not, this cell becomes the anchor. Nothing buzzes and nothing fails.
- `reveal(wordId)` is the bank writer. It shows that word, and on Tiny Tots and Little Pals shows its picture. It does not add the word to `found`. It does not speak.
- `useFallback(id)` loads the stored fallback for a group the check names. Play uses it only after the attempt cap. The fallback must pass the same uniqueness scan or the check fails.

`state()` adds `group`, `rows`, `cols`, `grid`, `anchor`, `found`, `words`, `directions`, `revealed`, and `harder`. `found` is a list of word ids. There is no miss count. `harder` is false unless the presence flag `harder` was set, and it changes directions only on Rush.

**Play, by band.** Cells fill the playfield. Spare pixels enlarge the cells. They do not add columns. The bank sits under the grid. Letters are uppercase. Twenty-six groups: four, six, eight, and eight.

- Tiny Tots. 4×3 fixed. Three words of three letters. Right only. At least one on-row chain, two words sharing a cell. Picture nouns. Four groups: pets, foods, body, toys.
- Little Pals. 5×4 fixed. Four or five words of three or four letters. Right only. One on-row chain. Pictures. Six groups.
- Shopkeeper. 6×6 fixed. Six to eight words of four or five letters. Right and down. Crossings, without a word embedded in another. Eight groups.
- Rush. 6×6, and 7×6 only if the cells stay at or above 72 px. Eight words of four to seven letters. Right, down, down-right, and down-left. Backward stays off. Eight groups.

The `harder` presence flag, on Rush only, adds left, up, up-left, and up-right. The value of the flag is ignored. Hygiene and the eight-direction scan include those directions before a harder grid is shown. Without the flag, a backward span does not score.

List hygiene runs before placement. An illegal list is an authoring error, not a play-time retry. Uppercase A–Z, one token, no duplicates, and neither word is a contiguous substring of the other or of the other's reverse. A palindrome is one span, not two. After placement, and again after filler, every answer has exactly one span in all eight directions, including directions the band does not play. Filler may not complete an answer. Up to 30 fill restarts, then backtrack. Up to 8 shuffles, then the stored fallback. The page never shows a grid that failed the scan. Right-only groups must share at least one cell. A unique grid is not thrown away only because the overlap ratio missed a target.

Marking. The cell under the pointer rests highlighted. That highlight is not a choice. Staying 650 ms anchors it. A ring fills during that wait. A second 650 ms dwell on the same cell clears the anchor. While an anchor is set, cells on the straight allowed line may preview-light when they are a prefix of one unfound word. The preview does not commit. Looking off the line clears the preview only. A commit that is not an answer becomes the new anchor. A bank chip is a menu dwell, 1300 ms, and only reveals. After a long idle, one unfound word may pulse its first cell once. It does not pulse at the start. Reduced motion skips the pulse. The pulse is not a commit.

Nothing in the game drags. A pointer down on one cell and up on another is not a stroke. Two taps are two commits.

**README.** The section says the child looks at the first letter and the last letter, and the word between them locks when those two looks match. It says a drag is not required. It names the bands by direction: across, then down, then forward diagonals. It says holding a word in the bank shows it, and does not by itself find it. The gaze paragraph names the cells as the play targets and the bank, Back, and the group chips as the slow ones. It does not mention speech.

**Self-check, after the family cases.**

1. `hygiene`. A fixture list with a word inside a longer word, or a word and its reverse, is refused and is not shown. Every shipped group passes.
2. `one-span`. On a pinned grid, each answer has exactly one eight-direction span. A palindrome, if a shipped group has one, counts once.
3. `chain-tiny`. The pinned Tiny Tots grid has two words that share a cell, and every word reads to the right.
4. `anchor-clear`. A 640 ms look does not set `anchor`. A 660 ms look does. A second 660 ms look on that same cell clears it.
5. `commit-either-order`. Anchoring the first cell and committing the last adds that word to `found` once. Anchoring the last and committing the first does the same. A second commit does not add it again.
6. `not-a-word`. A segment that is not an unfound answer leaves `found` unchanged, sets `anchor` to the new cell, and does not create a miss field. The banner does not say fail.
7. `preview-no-commit`. Moving the look along a legal line, without a second full dwell, does not add to `found`.
8. `bank-not-found`. A 600 ms look on a bank chip does not reveal. A 1300 ms look sets `revealed` to that word, leaves `found` unchanged, and on Tiny Tots shows the picture. `said` is not a field.
9. `shop-no-diagonal`. A diagonal span on Shopkeeper does not enter `found`.
10. `harder-off`. On Rush, without the flag, a backward span does not enter `found`. With the flag, the allowed set includes the four backward directions and the scan still has one span per word.
11. `fallback-unique`. `useFallback` on one group per band passes the same one-span scan and is the grid `state()` shows.
12. `no-drag`. A hold on one cell and a release on another does not add to `found`. Two taps do, when those cells are the ends of a word.
13. `cell-size`. Cell buttons from `targetsAt(390, 844)` have a short side of at least 72. Rush does not use 7×6 on that viewport if those cells would be smaller.
14. `no-speech`. Nothing calls `speechSynthesis`.
15. `group-reject`. `group` of an unknown id does not change the screen and does not insert that id into the page.

**Threats, beyond the shared list.** This is the game most likely to grow a speech call and a string built from the grid. The review checks that `speechSynthesis` is absent, that bank text is an authored word assigned with `textContent`, that filler letters are single alphabet characters, and that a query value cannot become a group id, a grid letter, or a revealed word. The `harder` flag is a boolean. Its value is not written into the page. The fallback cannot bypass the uniqueness scan.

## What later specs still decide

The later files under `docs/superpowers/specs/` choose the exact banner sentences, the canvas drawings, the Shopkeeper and Rush word lists inside the bounds above, the pinned seeds the checks will freeze, and the length of the flash-card show beat. They do not choose a new folder, a new window name, a new screen writer, a new dwell, a new socket, a speech call outside Flash Cards, or a second script.

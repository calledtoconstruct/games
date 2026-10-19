# Condensed word-search grids for gaze, touch, and mouse

A child can mark a word without dragging. Generate a small grid, place a hand-authored word group so letters are shared, and commit a find by dwelling the first cell and the last cell. Mouse click and touch tap are the same two-cell act with no wait. Play dwell stays about 650 ms. Menu and “speak this word” dwell stays about 1300 ms.

That marking model is inferred. No fetched source describes a gaze word search. It follows from dwell-to-select research and from the constraint that a gaze cannot drag a thin diagonal. Grid sizes below are smaller than typical print worksheets on purpose: child gaze studies needed targets several centimetres across, and a laptop playfield cannot hold a 15×15 of those cells.

## What “condensed” means

**Established.** A valid placement stays inside the grid and may cross another word only on equal letters. [Lefteris Moussiades, “A Word Search Puzzle Construction Algorithm” (IJSES, 2022)](http://ijses.com/wp-content/uploads/2022/01/68-IJSES-V6N1.pdf) calls a puzzle dense when placed letters cover more than half the cells. His C++ trials on an i7:

- 14 words on 11×11, about 58% of cells occupied: mean about 0.004 s over 100 runs, max about 0.03 s. One row smaller (10×11) jumped the max to 31 s.
- 34 longer words on 23×23, about 54% occupied: min about 0.023 s, max about 3.7 s. The paper’s stated mean of “0.093 centiseconds” is smaller than that minimum, so the average unit is inconsistent and should not be used.
- 12 words on 8×9, about 80% occupied: mean about 0.10 s, max about 4.9 s. Smaller boards failed outright.

He treats boards past about 50% as dense enough for educational use, and boards near 80% as the edge where a one-row shrink becomes a long stall. His search allows overlap; it does not seek it. Exhaustive enumeration is not viable: he sketches 15 words on 20×20 in two directions as on the order of 10^42 placements, and returns the first valid random placement instead.

**Claimed, via Moussiades’s review of other builders, not re-read here.** A patent method places the first word at random and then keeps a later word only when it overlaps an existing letter, with a second pass for skips and a full restart if anything remains. Moussiades says that is acceptable when the board is roomy and delay-prone when it is dense. A 2020 language-learning builder he cites puts at most one word in each row or column, which avoids the overlap this game wants. [Jamis Buck’s 2015 generator](https://weblog.jamisbuck.org/2015/9/26/generating-word-search-puzzles.html) backtracks through shuffled cells and directions and accepts a crossing when the letter already there matches. His [command-line tool](https://github.com/jamis/wordsearch) defaults to 15×15 and warns that too many words in too small a grid “may take a very long time, and will probably fail.”

**Inferred.** No source defines an overlap count. Use two measured ratios, and treat Moussiades’s 50–65% band as the comfortable dense range:

- Occupancy = cells that hold a placed letter, divided by all cells. Aim for 0.50–0.65 before filler. Do not chase 0.80.
- Overlap ratio = sum of word lengths, divided by occupied cells. 1.0 means no shared cell. Aim for at least 1.15 when the direction set can cross (down or diagonal). Right-only overlap is a chain on one row; require at least one such chain in those groups, not a global 1.15 if the list has no chain.

Prefer shared letters. Do not fail a finished, unique grid only because a list of unrelated letters could not overlap.

## Placement that stays small

**Established.** Both practical algorithms are depth-first backtracking with a shuffled candidate list. Buck stores grid, remaining directions, and remaining cells on a stack, pops a failed word, and restores the previous grid. Moussiades adds a tested-location set so a word is not put back on a cell already shown not to leave room for the next word. Both stop at the first success. Both run in list order, not longest-first. Diagonals are a direction flag, not a different algorithm: Buck’s base set is right and down; diagonal and backward are options. Moussiades tested horizontal and vertical only and says other directions can be added the same way.

**Inferred for this game.** Sort longest word first so the span that most needs room is placed while the grid is empty, then place shorter words onto those letters. Score each legal cell and direction by how many letters it shares, shuffle inside a tied score, and try high scores first. A zero-score placement stays legal, or a list with no shared letters could never finish. Cap the search. Moussiades’s tail of 31 seconds is a bad play-time wait: stop around 20,000 attempts or about 50 ms, grow the grid by one row or one column, and restart. Growth is capped by the age band so cells stay large. If the cap is hit, use a fallback layout stored with the group. Authoring must produce that fallback with the same checker, so play never dead-ends.

Buck fills the holes with a uniform draw from a–z and does not look at what those letters spell. That fill is the wrong default here.

## One occurrence of each word

**Claimed.** [Reading Vine’s word-search maker](https://www.readingvine.com/collections/word-search-maker/) (page dated 2026-08-09) says that after placement it checks filler in all eight directions and re-rolls any spot that spells a real word, including words that should not appear on a classroom page. That is a vendor claim about a private dictionary, not a published procedure. Buck and Moussiades do not do it.

**Inferred rules that actually match “each answer string at most once.”**

1. List hygiene, before any placement. Uppercase A–Z, one token, no duplicates. For two different words, neither word may be a contiguous substring of the other, or of the other’s reverse. That rejects CAT inside CATCH, and RAT together with TAR. A palindrome such as MOM is allowed: the reverse read is the same cells.
2. After placement, before filler, scan every start cell in all eight directions, including directions the band does not use for play. Each answer must have exactly one span. The span is the set of cells, so the forward and backward read of a palindrome count once. A second span means two crossings spelled a listed word; backtrack.
3. Fill empty cells from A–Z, but accept a letter only when it does not complete an answer through that cell. This incremental test misses a word that still has another empty cell on its span. The real gate is a second full eight-direction scan after the grid is full. Up to 30 fill restarts, then throw the placement away and backtrack.
4. Do not require “no English word except the list.” Three-letter windows spell real words constantly, and a dictionary re-roll like Reading Vine’s will stall a Tiny Tots grid. A short denylist of slurs is a separate, optional filter.

Failure modes the checker exists for: a short word hiding in a longer one; a reverse pair; filler completing a three-letter answer; two placed words aligning into a third answer; counting a palindrome twice; checking only the play directions and missing a backward accident.

## Gaze marking

**Established.** Dwell is the usual way to tell a look from a choice. A 2024 methods preprint on gaze-interaction simulation reports prior dwell optima spread from 150 ms to 1000 ms, and notes that a shorter dwell raises accidental selection ([arXiv:2411.03708](https://arxiv.org/html/2411.03708v1), search result dated 2024-11-06). [Liao and colleagues (MDPI, © 2022)](https://www.mdpi.com/2220-9964/11/2/127) had 38 adults select map features with a Tobii 4C at about 50 cm, where 1° is about 0.87 cm and the tracker’s stated accuracy is 0.5°. Dwells of 200, 600, and 1000 ms: 600 ms was more efficient than both neighbours and less accurate than 1000 ms; they preferred 600 ms because the longer dwell risks visual fatigue. A point buffer of 1.5° beat 1° and 2°. Larger buffers were faster and less accurate. They also cite earlier work in which a 600 ms dwell was rated easiest, and in which a 256 px button was more accurate than 128 px.

Children and users with physical impairment needed much larger targets. [Sakamaki and colleagues (DOI 10.1080/17483107.2020.1729874)](http://www.ece.ualberta.ca/~tbs/pmwiki/pdf/DRAT-Sakamaki-2020.pdf) fixed dwell at 1.5 s. Non-disabled adults were tested at 3, 6, and 9 cm. Children and participants with cerebral palsy were tested at 6, 9, and 12 cm, because a 3 cm target was already too hard in pre-tests. Younger children were slower. Feedback that shows where the tracker thinks the gaze is made selection faster and more accurate; no single feedback modality won. [Borgestig and colleagues](https://pmc.ncbi.nlm.nih.gov/articles/PMC4867850/) followed 10 non-speaking children with severe physical impairment, ages 1–15, selecting 100×100 px targets at a 1 s dwell. Time on task improved by about 5 months; accuracy improved only at 15–20 months. The devices they used quote about 0.5–0.8° of gaze error. [Walker and Wegner (ATIA, Winter 2021)](https://www.atia.org/wp-content/uploads/2021/03/V15_WalkerWegner.pdf) taught one child with cortical visual impairment to dwell-select, found 300 ms the best of the dwells they tried for him, and skipped drag-and-drop training because the communication icons were already big enough and the software did not need a drag.

The product’s 650 ms play dwell sits on Liao’s adult balance point and inside the 150–1000 ms range. It is shorter than Sakamaki’s 1.5 s and Borgestig’s 1 s, so cell size has to do more of the work. Menu dwell at 1300 ms is near those longer child dwells and should stay off the letter cells.

**Inferred model.** Commit by dwelling two cells: the first letter and the last letter, in either order. The engine takes the straight segment between them. A diagonal is then one line between two large targets, not a dragged stroke. Reject path-following as the commit. Gaze jitter skips cells, and Walker’s case is explicit that drag is an extra skill. Also reject “dwell the word in the list” as the way to score the find: that marks the bank chip without showing the word was seen in the grid. A bank dwell of 1300 ms may only speak the word.

A preview may light the cells between the anchor and the cell currently looked at when they lie on one allowed line and match the start of an unfound word. The preview is feedback in the sense Sakamaki found useful. It must not be required, and it must not commit, or scanning the grid becomes a stream of accidental finds (Liao’s Midas touch).

**Cell size, inferred from the conflict above.** Liao’s adults could use a buffer near 1.3 cm. Sakamaki’s children could not use 3 cm. Prefer cells near 6 cm, and treat under about 3 cm as the size that already failed a child pre-test. A 15×15 cannot do that on a laptop. [One developer who tried 25×25](https://zolmok.org/wordsearch-puzzle/) found the words lost in filler and settled on 15×15; that anecdote is about visual search on a large grid, and it points the same way as the gaze constraint: do not grow the grid to make the puzzle harder.

## Directions and sizes by age

These guides are teacher and worksheet sites, not trials. They agree on the order of difficulty and disagree on the birthday when diagonals start.

**Claimed, and consistent on order.** Rightward is the early-reader direction. Down is next. Diagonals fight the left-to-right scan children are still learning. Backward is last. [Quixword (2026-03-03)](https://www.quixword.com/blog/word-search-for-kids): ages 5–7 use 8×8 to 10×10, 6–8 words, across and down, with 3–5 letter words; ages 7–10 add limited diagonals on about a 10×12 to 12×12 with 8–12 words; ages 10–12 use all directions including backward. They warn that a 15×15 for only eight words feels endless, and that seven-letter words in a small grid make overlaps young eyes cannot sort. [Montessori Activities at Home (2026-03-27)](https://montessoriactivitiesathome.com/blog/best-educational-word-searches-kids): ages 4–5 are 6×6 or 8×8, 4–6 words, 3–5 letters, left to right only; ages 5–7 add down on 10×10 or 12×12; ages 7–9 add diagonals; ages 9–10 add backward on 15×15 or larger. [Cluegrid (2026-06-10)](https://cluegrid.org/guides/how-many-words-should-a-word-search-have): kindergarten–grade 1 is 6–8 words across and down; grades 2–3 are 10–12 words; grades 4–5 may use diagonals at 12–15 words; grades 6–8 add backward at 15–20 words. [Junosphere (2026-04-22)](https://junosphere.in/parents-corner/free-printable-word-search-kids-preschoolers) never uses diagonals in its 4–7 sheets: 9×9 and 8 words at ages 5–6, 11×11 and 10 words at 6–7, 13×13 at 7+, always across and down. A four-year-old is matching letter shapes with an adult, one or two words at a time, after the word is read aloud. [Reading Vine](https://www.readingvine.com/collections/word-search-maker/) maps the same ladder onto a control: easy is across and down, medium adds diagonals, hard is all eight, and Auto picks the smallest grid the words “fit comfortably.” [PuzzleGenio’s special-education notes (2026-01-20)](https://puzzlegenio.com/posts/word-search-activities-for-special-education) start at horizontal only, 3–5 words, pictures, and a pre-highlighted first letter, then add down, then all directions. The same site’s kids page suggests 10×10 for ages 5–7 and 15×15 for ages 8–12, which is the large-grid side of the disagreement.

**Decision for this game.** Follow the stricter guides, and also refuse diagonals wherever a miss would require dragging.

- Tiny Tots, about 2–3: right only. No fetched guide offers an independent word search this young. Picture plus three-letter noun is extrapolated from Junosphere’s four-year-old practice and PuzzleGenio’s pictured horizontal start.
- Little Pals, about 4–5: right only. Montessori keeps this age horizontal. Junosphere adds down at five. Down is not the band default.
- Shopkeeper, about 6–8: right and down. Diagonals wait. Cluegrid holds them to about grade 4, Junosphere still omits them at seven, and a diagonal is the gaze-fragile direction even when the commit is two endpoints.
- Rush, about 9+: right, down, and the two forward diagonals (down-right, down-left). Backward stays off. Cluegrid adds backward at about grades 6–8, not at age nine. An optional harder flag may turn on the four backward directions for older Rush players. Endpoint commit is what makes those diagonals fair; do not implement them as a drag.

Print-worksheet word counts (often 8–15) do not fit 6 cm cells. This game uses fewer words on a smaller grid and gets difficulty from direction and overlap, which is the tradeoff Quixword recommends when the alternative is a bigger grid.

## Gaps

- Beelders’s 2023 eye-tracking pilot on word-search puzzles appeared in search (PMC10644992) and in a later citation, but the fetch returned only the PMC title. None of its findings are used.
- The Taylor & Francis dwell-time paper and a Stack Exchange generator were bot-blocked.
- Academic search returned crossword solvers, not word-search constructors. Moussiades was found on the open web.
- No source states an overlap ratio. 1.15 is a design threshold.
- No source tests children’s gaze at 4 cm or at 650 ms. The 6 cm aim is Sakamaki’s smallest successful child size, at a longer dwell.
- Age tables are vendors and teachers. They are not independent studies, and Junosphere, Quixword, and Montessori disagree on diagonals by about two years.
- Reading Vine’s “no accidental real words” claim was not reproduced from code.

## Generator specification

### Inputs

- A group: theme name, home age band, and a list of original common nouns. No copied published puzzle.
- The band profile below: start grid, max grid, directions, word count, length.
- A seed. Play uses a new seed each time the group is chosen. Tests may pin one.
- One fallback layout per group, produced by this same spec at authoring time.

### Band profiles

| Band | Start grid | Max grid | Words | Letters | Directions |
| --- | --- | --- | --- | --- | --- |
| Tiny Tots, ~2–3 | 4×3 | 4×3 | 3 | 3 | right |
| Little Pals, ~4–5 | 5×4 | 5×4 | 4–5 | 3–4 | right |
| Shopkeeper, ~6–8 | 6×6 | 6×6 | 6–8 | 4–5 | right, down |
| Rush, ~9+ | 6×6 | 7×6 | 8 | 4–7 | right, down, down-right, down-left |
| Rush, harder flag, ~11+ | same grid | same grid | same list | same lengths | those four, plus left, up, up-left, up-right |

Cells fill the playfield. Do not add columns to use spare pixels; enlarge the cells. Prefer about 6 cm. Avoid going under about 3 cm, the size Sakamaki’s child pre-test could not select. A 64 CSS-pixel floor is only a touch minimum, not a gaze success size. The word bank sits under the grid, not beside it, so it does not steal width (Quixword’s print advice, applied here to the playfield).

**Revised 2026-10-19.** The paragraph above is the 2026-10-18 finding. It is not the layout rule. A look is a point on the screen. A letter that has scrolled away cannot be selected, so the page must not scroll. The cell is the largest size that keeps the chrome, the full grid, the bank, and a revealed picture inside the viewport. Spare width does not grow a cell once the height is used. On a laptop near 2000 by 1100 CSS pixels, a 6 by 6 grid plus the bank cannot also be 6 cm cells, and the fit wins. The 6 cm preference remains the reason to take the largest cell that still fits, and to cap growth near 168 CSS pixels so a wide window does not turn one letter into a tile that pushes the bank off the glass. Grid sizes, word lists, overlap, and the denylist are unchanged.

### Placement order

1. Normalize and run list hygiene. An illegal list is an authoring error, not a play-time retry.
2. Sort longest first. Break remaining ties at random.
3. Start from the band’s start grid, all cells empty.
4. For the current word, list every in-bounds start cell and every allowed direction. A candidate is legal when every filled cell it touches already equals the letter it needs. Score by the count of those shared cells. Shuffle ties. Try higher scores first.
5. On success, go to the next word. On failure, mark this word’s tried starts cleared, step back one word, and forbid the start that word just used (Moussiades’s tested-location set). If the first word has no unused start, this grid has failed.
6. Stop the search at about 20,000 attempts or about 50 ms. Grow by one row, then one column, up to the band max, and restart from step 3. Tiny Tots through Shopkeeper have no room to grow; a failed search there goes straight to the fallback.
7. When every word is placed, run the pre-fill uniqueness scan. A second span backtracks the last word.
8. If occupancy is above about 0.70, or the overlap ratio is below 1.15 on a band that allows down or diagonal, do not discard a unique grid that already fits. Prefer a higher-scoring search next time by keeping the shared-letter sort. Right-only groups must include at least one on-row chain (the end of one word is the start of another, as in CAT beside ATE on the row CATE). That chain is an authoring requirement, checked by seeing that at least two words share a cell in the finished grid.

### Uniqueness check and retries

1. Hygiene, as above. One span of cells counts once, so palindromes are not double occurrences.
2. Pre-fill eight-direction scan: each answer has exactly one span.
3. Filler: walk empty cells in random order. Try letters in random order and keep the first letter that does not finish an answer through that cell. Then scan all eight directions again. Each answer has exactly one span.
4. Up to 30 fill restarts per placement. Then backtrack placement.
5. Up to 8 full restarts from a fresh shuffle at the current grid size. Then grow if the band allows, else use the stored fallback.
6. Never show a grid that failed step 2 or step 3. There is no lose state if the child is slow, and there is no broken grid if the generator is slow.

### Dwell marking

1. Mouse, touch, and gaze share one pointer and one commit. Gaze moves the pointer continuously. A click or a tap is a commit with zero wait. Gaze commit waits 650 ms. Nothing in the game drags.
2. The cell under the pointer rests highlighted. That is not a choice.
3. Staying in one cell for 650 ms anchors it. A ring fills during the wait. A second 650 ms dwell on that same cell clears the anchor.
4. While an anchor is set, cells on the straight allowed line from the anchor to the current cell may preview-light if they are a prefix of one unfound word. Looking away, or a jitter off the line, only clears the preview.
5. A 650 ms dwell on a second cell commits when the segment between the two cells, either order, is exactly one unfound answer in an allowed direction. The whole word locks highlighted, and its bank chip is marked. If the segment is not an answer, do not buzz and do not fail: the new cell becomes the anchor.
6. A 1300 ms dwell on a bank chip speaks that word and, for Tiny Tots and Little Pals, shows its picture. It does not mark the word found.
7. After a long idle, one unfound word may pulse its first cell once. Do not pulse at the start of the round. PuzzleGenio’s always-on first-letter highlight is a stronger scaffold than this game needs.

### How the 20–30 groups are shaped

Author 26 groups, inside the 20–30 range, each locked to one home band. Each time it is selected, the engine re-places it. The fallback is only for a generator miss.

- Tiny Tots: 4 groups × 3 picture nouns, exactly 3 letters, right only. At least one on-row chain. Themes a toddler can say: pets, foods, body, toys. Pictures on the chips.
- Little Pals: 6 groups × 4 or 5 words, 3–4 letters, right only, one on-row chain, still pictured.
- Shopkeeper: 8 groups × 6–8 words, 4–5 letters, right and down, letters shared across words so crossings exist (CAT, CAR, RAT style, without a word embedded in another).
- Rush: 8 groups × 8 words, 4–7 letters, forward diagonals included. Avoid reverse pairs. Palindromes are allowed. Do not turn backward on for these lists until the harder flag, and re-run hygiene and the eight-direction scan with backward included before enabling it.

Keep every word a common noun a child can picture. Quixword’s reasons: concrete themes, no near-homonyms (BEAR next to BARE), no inflected form when the child knows a different one. Do not reuse their holiday lists, Junosphere’s beach list, or any other published grid. Within a group, pick words that share letters so the scorer has something to overlap. Across groups, themes can repeat only with a new list.

Uppercase everywhere. Early bands may show the bank word in lowercase as a second line if the product later wants letter-shape practice; Reading Vine’s claim is that K–2 students are still learning lowercase. The grid itself stays uppercase so gaze targets are one stable glyph.

sources_reviewed: 80

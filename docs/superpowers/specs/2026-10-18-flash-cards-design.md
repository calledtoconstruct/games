# Flash Cards

**Date.** 2026-10-18. Layout revised 2026-10-19.

**Status.** Build spec. One implementer should be able to ship the page from this file alone. The 2026-10-19 layout note below replaces the horn prompt, the two-picture Tiny deal, the prompt-on-top stack, and the 2×2 four-card grid. Speech, the miss beat, and the dwell times stay where they do not describe the horn or a two-card deal.

**Folder.** `flash-cards`

**File.** `flash-cards/index.html` only. No second script, no module, no asset folder, no SVG file, no credits file.

**Title.** Flash Cards

**Layout revised 2026-10-19.** One prompt card sits on the left. The answers are a column on the right: three for Tiny Tots and Little Pals, four for Shopkeeper and Rush. `picture-word` shows the picture on the prompt and does not make that card a target. `hear-picture` and `word-picture` show the printed word on the prompt. Dwelling that word speaks it and does not choose. There is no horn on the ask screen. The menu line is "Match the card on the other side." The picture-word banner stays "Look at the picture, then the word." The other two directions say "Look at the word, then the picture." A white shape is drawn with a `#7a6552` contour so a sheep, a cow, an egg, or the moon stays visible on the paper card. The frozen two-choice Tiny lists earlier in this file are retired. `tiny-three-pictures` and `engine-tiny` in the page are the three-choice lists.

**Window.** `window.flashCards`, assigned as soon as the script runs, before the query string is handled.

**License.** The repo MIT license. Copyright 2026 Joseph Woolley. This file adds no second license.

This page copies the gaze client behavior from `saddle-up/index.html`. It does not import it. Constants, hello bytes, framing, and the pointer rule match the shared contract below. The only intended client difference is which button a finished dwell activates, plus the leave grace this spec defines for `LEAVE_MS`.

## Player loop

One sitting is one subject. The child never sees a score, a miss count, a life, a rank, or a clock.

1. The menu asks who is playing. The child picks Tiny Tots, Little Pals, Shopkeeper, or Rush, then Let's go.
2. The subject screen lists only that band's subjects. The child picks one. Stop can leave before a subject is picked.
3. A trial appears. Nothing is spoken just because the trial appeared. The prompt and the choices depend on the direction:
   - `hear-picture`. A horn is the prompt. The choices are pictures. The child looks at the horn until it finishes, hears that one word, then looks at the matching picture. Looking at the horn does not pick a card.
   - `picture-word`. The prompt is a picture and it is not a target. The choices are printed words. The child looks at the matching word.
   - `word-picture`. The prompt is a printed word. Looking at it hears that word and does not pick a card. The choices are pictures.
4. A finished look or a tap on a choice card selects it. There is no drag.
5. Right or wrong, the page shows the right picture and the right word together and outlines the right card. It does not say wrong, fail, or no. It does not add a score. It does not speak the answer by itself.
6. After 800 ms of `step` time the next trial deals, or the sitting ends on the rest card.
7. The sitting also ends when the trial cap is finished, when the hidden minute ceiling is reached, or when Stop is used. Rest has no grade. Another deals the same band again, back on the subject screen.

Card positions shuffle when the trial is dealt and stay through that trial's show. A miss does not replay the trial, so there is no second shuffle to avoid.

Tiny Tots is only step 3's first bullet, two pictures, no printed choice. Shopkeeper and Rush use the other two directions on the schedule in Generator.

## Bands and subjects

Bands are not renamed. Rush stays. There is no fifth band.

Ages: Tiny Tots about 2–3, Little Pals about 4–5, Shopkeeper about 6–8, Rush about 9 and up.

One subject per sitting. Foils come from that subject's list only. Printed words are the id in lowercase. Pictures have no letters. Every id below is an authored word. Nothing else is ever spoken or printed as a card.

| Band | Id | Choices | Directions | Deck | Trials | Hidden ceiling |
| --- | --- | --- | --- | --- | --- | --- |
| Tiny Tots | `tiny` | 2 | `hear-picture` only | 6 | 8 | 4 minutes |
| Little Pals | `little` | 3 | `hear-picture`, except the print budget | 8, or 6 for colors | 12 | 8 minutes |
| Shopkeeper | `shop` | 4 | `picture-word`, and 5 of 16 `word-picture` | 10 | 16 | 12 minutes |
| Rush | `rush` | 4 | 8 of 16 `picture-word` and 8 of 16 `word-picture` | 12, or 4 for time | 16 | 15 minutes |

The ceilings are 240000, 480000, 720000, and 900000 ms of counted `step` time. They are not drawn. Caps are ceilings. Stop may end a sitting on the first trial. Time uses the Rush cap of 16, not 8.

Subject buttons show the display name. The id is what `subject` accepts. An id from another band is not on the menu and is rejected if passed in.

### Tiny Tots

No colors, places, or time words. No printed choice on the ask. Deck of 6, so two words are foils only in a sitting of 8.

**Animals** (`animals`). dog, cat, bird, fish, cow, duck.

**Food** (`food`). milk, apple, banana, bread, cookie, water.

**Body** (`body`). eye, nose, mouth, hand, foot, tummy.

### Little Pals

These seven subjects. Animals, food, and body grow by two words. Colors are spoken pictures only.

**Animals** (`animals`). dog, cat, bird, fish, cow, duck, pig, horse.

**Food** (`food`). milk, apple, banana, bread, cookie, water, egg, carrot.

**Body** (`body`). eye, nose, mouth, hand, foot, tummy, ear, hair.

**Clothes** (`clothing`). hat, shoe, sock, coat, shirt, pants, dress, boot.

**Toys** (`toys`). ball, doll, block, book, drum, kite, top, bell.

**Vehicles** (`vehicles`). car, bus, bike, boat, plane, train, truck, wagon.

**Colors** (`colors`). red, yellow, blue, green, black, white.

### Shopkeeper

Deck of 10, including colors. Color names may be printed. Foils may be neighbors.

**Animals** (`animals`). dog, cat, cow, horse, sheep, goat, pig, frog, mouse, duck.

**Food** (`food`). apple, banana, bread, cheese, egg, carrot, corn, grape, milk, soup.

**Household** (`household`). cup, spoon, bowl, plate, fork, pan, pot, key, clock, phone.

**Furniture** (`furniture`). bed, chair, table, desk, sofa, lamp, shelf, rug, door, crib.

**Clothes** (`clothing`). hat, shoe, sock, coat, shirt, pants, dress, boot, glove, scarf.

**Vehicles** (`vehicles`). car, bus, bike, boat, plane, train, truck, wagon, sled, cart.

**Outside** (`outside`). tree, flower, rock, sun, moon, star, rain, cloud, grass, leaf.

**Colors** (`colors`). red, yellow, blue, green, black, white, orange, purple, brown, pink.

Orange on this list is the color, not a fruit.

### Rush

**Kitchen** (`kitchen`). ladle, whisk, grater, kettle, oven, sink, tray, mixer, scoop, sieve, mitt, peeler.

**Weather** (`weather`). rain, cloud, sun, wind, snow, storm, fog, hail, rainbow, puddle, umbrella, frost.

**Places** (`places`). home, school, park, farm, store, beach, hill, lake, bridge, road, garden, library.

**Animals** (`animals`). owl, fox, whale, crab, ant, bee, seal, toad, hare, moth, crane, dolphin.

**Colors** (`colors`). red, yellow, blue, green, black, white, orange, purple, brown, pink, gray, tan.

**Time** (`time`). morning, noon, evening, night.

Time is four words and four choices, so every trial shows the whole deck. The plan still runs 16 trials. Today and tomorrow are not words in this game.

## Drawing notes

Every picture is an original canvas drawing on `#view`, keyed by the word id. One object, flat fills, high contrast. No letters, no digits, no logo, no brand name, no trademark, no character, no downloaded file, no remote image. A color chip is a flat disk. That is the only allowed non-object shape. A word with no painter is a failed check, not a network fetch. The same id always uses the same painter, on every band.

The canvas is full-page, `pointer-events: none`, and is not a gaze target. It is redrawn from `state()` on every `render`. Buttons sit above it and are transparent where the drawing shows through. Picture buttons have empty `textContent`. An `aria-label` of the lowercase word is allowed and is not a printed choice.

Do not draw a face on the sun, the moon, the doll, or the clock. Do not draw a collar, clothes on an animal, a rider, a label, or an icon.

Palette, used when a row names a color and does not name a hex: ink `#2c2416`, cream `#f4f0d8`, paper `#fffdf6`, brown `#8a5a2b`, dark brown `#5c3a1e`, tan `#d7b07a`, orange `#e07a2f`, yellow `#f2c14e`, red `#d64545`, pink `#f2a3b5`, blue `#3a7bd5`, green `#3f8f4e`, purple `#7a4ea3`, gray `#9aa3ad`, metal `#c5ccd6`. Skin for hand, foot, tummy, ear, and the doll head is `#f0c9a0`.

### Animals and body

| Id | Drawing |
| --- | --- |
| dog | Side dog, brown, one floppy ear. No collar. |
| cat | Side cat, orange, pointed ears, curved tail. No collar. |
| bird | Side bird, blue, one wing, one beak. |
| fish | Side fish, orange, one tail fin. |
| cow | Side cow, white, exactly two black spots. No brand. |
| duck | Side duck, yellow, flat bill. |
| pig | Side pig, pink. No clothes. |
| horse | Side horse, brown, dark mane. No rider. |
| sheep | Side sheep, white, four legs. |
| goat | Side goat, tan body, two short horns. |
| frog | Side frog, green, four legs. |
| mouse | Side mouse, small, gray, long tail. |
| owl | Front owl, brown, two round eyes, small beak. |
| fox | Side fox, orange, bushy tail. |
| whale | Side whale, gray, tail fluke. |
| crab | Top-view crab, red, two claws. |
| ant | Side ant, brown, three body parts, six legs. |
| bee | Yellow oval body, two black stripes, two wings. |
| seal | Side seal, gray, one flipper. |
| toad | Side toad, brown-green, wider than the frog, no horns. |
| hare | Side hare, tan, two long ears. Not a puppy and not the dog. |
| moth | Front moth, tan, two wide wings. |
| crane | Side crane, gray body, long legs, long beak. |
| dolphin | Side dolphin, gray, beak-like snout, distinct from the whale by the snout and a smaller body. |
| eye | One eye. White oval, dark blue iris, ink pupil. |
| nose | Front nose, simple, skin color. No face around it. |
| mouth | Two red lips. No teeth. |
| hand | Open hand, five fingers, skin color. |
| foot | Side foot, skin color, no shoe. |
| tummy | Round belly, skin color, one navel dot. No face. |
| ear | Side ear, skin color. Not on a face. |
| hair | Dark ink hair on a blank head oval. No eyes and no mouth. |

### Food, kitchen, household

| Id | Drawing |
| --- | --- |
| milk | Glass of opaque white milk. The glass is narrower than water. |
| water | Glass of blue-tinted water. The glass is wider than the milk glass. |
| apple | Red apple, brown stem, one green leaf. |
| banana | Yellow banana, curved. |
| bread | One bread slice, tan crumb, brown crust. |
| cookie | Round tan cookie, exactly three dark dots. |
| egg | One white egg. |
| carrot | Orange carrot, green top. This is the vegetable, not the color orange. |
| cheese | Yellow cheese wedge. No label. |
| corn | Ear of corn, yellow, partly in a green husk. |
| grape | Bunch of purple grapes. |
| soup | Bowl of orange soup. No label and no spoon. |
| ladle | Metal ladle, round cup, long handle. |
| whisk | Metal whisk, handle and a loop of wires. |
| grater | Flat metal grater, small holes, no brand. |
| kettle | Kettle with a handle and a spout. No brand. |
| oven | Box oven with a dark window. No brand and no digits. |
| sink | Side sink basin, gray. |
| tray | Rectangle tray, metal, empty. |
| mixer | Mixer standing in a bowl. No brand. |
| scoop | Short metal scoop. |
| sieve | Round sieve, metal rim, mesh fill. |
| mitt | One oven mitt, red. |
| peeler | Small peeler, handle and one blade. |
| cup | Blue cup. |
| spoon | Side spoon, metal. |
| bowl | Empty bowl, paper colored. |
| plate | Round plate, paper colored. |
| fork | Side fork, metal, three tines. |
| pan | Frying pan, dark gray, side view. |
| pot | Lidded pot, dark gray. |
| key | Side key, metal. |
| clock | Round clock, paper face, two ink hands. No numerals and no ticks that could be read as digits. |
| phone | Rectangle phone, dark body, darker screen. No logo and no icons. |

### Clothes, toys, vehicles, furniture

| Id | Drawing |
| --- | --- |
| hat | Yellow hat, tall crown, wide brim. No logo. |
| shoe | Side brown shoe, low. |
| sock | Blue sock with a cuff. |
| coat | Blue coat, front view. No logo. |
| shirt | Red shirt, front view. No logo. |
| pants | Blue pants. |
| dress | Red dress. |
| boot | Side brown boot, taller than the shoe, darker brown. |
| glove | One glove, red. |
| scarf | Scarf with red and yellow stripes. |
| ball | Blue ball, one white stripe. No logo. |
| doll | Round head, red dress, no eyes, no mouth, no character face. |
| block | Red cube. |
| book | Closed book, blue cover. No title and no letters. |
| drum | One drum with two sticks beside it. One drawing, not two cards. |
| kite | Red diamond kite, short tail. |
| top | Spinning top with red and yellow bands. |
| bell | Yellow bell. |
| car | Side red car. No text. |
| bus | Side yellow bus, square windows. No text. |
| bike | Side blue bike. |
| boat | Side boat, brown hull, one white sail. |
| plane | Side airplane, gray. No livery and no letters. The word is plane. |
| train | Side train engine, ink black. |
| truck | Side box truck, tan cargo box. |
| wagon | Side wooden wagon, four wheels. |
| sled | Side sled, red. No rider. |
| cart | Side cart, two wheels, distinct from the wagon. The wagon has four wheels and a wooden box. The cart has two wheels and a flat bed. |
| bed | Side bed, brown frame, blue blanket. |
| chair | Front chair, brown, four legs. |
| table | Side table, brown, four legs. |
| desk | Side desk, brown, flat top, four legs, distinct from the table by a thicker top. |
| sofa | Side sofa, blue, a back and two arms. |
| lamp | Lamp with a yellow shade and a thin stand. |
| shelf | Wall shelf, brown, empty. |
| rug | Rectangle red rug, darker red border. |
| door | Closed door, brown, one round knob. |
| crib | Crib with side rails. No mobile. |

### Outside, weather, places, time, colors

| Id | Drawing |
| --- | --- |
| tree | Green tree, brown trunk. |
| flower | One flower, red petals, yellow center, green stem. |
| rock | One gray rock. |
| sun | Yellow sun disk, eight short rays. No face. |
| moon | Crescent moon, paper colored. No face. |
| star | One five-point star, yellow. |
| rain | Five blue drops. Not a cloud. |
| cloud | One white cloud. |
| grass | One tuft of green grass. |
| leaf | One green leaf. |
| wind | Three curved horizontal ink lines. |
| snow | One six-arm snowflake, paper colored. |
| storm | Dark ink cloud with three blue drops under it. |
| fog | Three horizontal gray bands. |
| hail | Three white hail stones as one cluster. |
| rainbow | Six flat arcs, red, orange, yellow, green, blue, purple. No characters. |
| puddle | Blue puddle on a brown ground strip. |
| umbrella | Open red umbrella. |
| frost | One six-sided frost crystal, paper colored, distinct from the snowflake by the hexagon. |
| home | Silhouette of a house with a chimney. No letters. |
| school | Silhouette of a building with a bell tower. |
| park | Silhouette of a swing. |
| farm | Silhouette of a barn. |
| store | Silhouette of a shop with a striped awning. |
| beach | Silhouette of a striped umbrella in a sand mound. |
| hill | Green hill silhouette. |
| lake | Blue oval. |
| bridge | Arch bridge silhouette. |
| road | Gray road with one pale dash. The dash is not a letter. |
| garden | One flower bed, brown soil, three red blooms. |
| library | Building with an arched door and steps. No bell and no letters. |
| morning | Low yellow sun on a light blue sky. The sun is not a separate card inside this drawing. |
| noon | High yellow sun on a bright blue sky. |
| evening | Low orange sun on a purple sky. |
| night | Crescent moon on a dark blue sky, and two small stars in that same drawing. |

Color disks are centered on the card. Red `#d64545`, yellow `#f2c14e`, blue `#3a7bd5`, green `#3f8f4e`, orange `#e07a2f`, purple `#7a4ea3`, brown `#8a5a2b`, pink `#f2a3b5`. Black `#1b1b1b` sits on a cream card so the edge shows. White `#fffdf6` has a thick ink rim, about one eighth of the radius. Gray `#9aa3ad` is lighter than black. Tan `#d7b07a` is lighter than brown. No word is painted on a disk.

Place drawings and the weather scenes are silhouettes or flat shapes with no letters. Kitchen tools have no labels.

## Screens and API

### Page skeleton

One HTML document. No frame, no second script, no remote image, font, style sheet, or beacon. The head holds charset, a viewport meta, theme color `#f4f0d8`, and the title Flash Cards. Nothing else is loaded.

The body, in this order:

1. `#view`, the full-page canvas, draw only.
2. Chrome that is always in the tree: `#mute`, `#full`, `#back`, `#eyes`, `#gaze`, `#banner`.
3. `#screen-menu`.
4. `#play`, which holds the subject buttons, the trial buttons, and Stop.
5. `#screen-rest`.
6. The self-check node is not in the authored markup. The loader creates a `pre` with id `selfcheck-result` only when `selfcheck` is present.

`#mute` is a 72 px button with two inline speaker SVGs, one hidden at a time. It has no visible text Mute or Sound. The accessible name may say "Mute sound" or "Turn sound on". `#full` is a 72 px Fullscreen button. `#back` says Back to Menu, is a menu dwell, and is hidden on the menu. `#banner` is one short sentence. It does not receive clicks. It is not a score, a timer, or a rank. `#eyes` is the chip that shows the word Eyes. It is not a button. `#gaze` is the ring.

Font stack: Arial Rounded MT Bold, then Trebuchet MS, then Segoe UI. Cream cards `#fffdf6` on paper `#f4f0d8`, ink `#2c2416`, sun-gold primary `#ffc857`. The ring is green `#2f8a3e`.

`go` shows one screen and hides the others. `go` is the only writer of the screen id.

### Menu

Title Flash Cards. One line: Hear a word, then match the picture. Then the line Ages 2 and up. Four band buttons, labels Tiny Tots, Little Pals, Shopkeeper, and Rush, ids `tiny`, `little`, `shop`, and `rush`. A sun-gold Let's go button. Band buttons and Let's go are menu dwells. They do not carry `data-play`. The first highlighted band is Tiny Tots. Picking a band highlights it. Let's go calls `start` with the highlighted id.

Banner: `Pick who is playing.`

### Subject

`start` lands here. Buttons are the display names for that band only, in the order listed in Bands and subjects. They are menu dwells. Choosing one calls `subject(id)`. Stop is here too and calls `go("rest")`. Back calls `go("menu")`.

Banner: `Pick a subject.`

### Trial

Dealt by `subject`. Phase `ask`, then `show`.

Ask banners, exact text:

- `hear-picture`: `Look at the horn, then the picture.`
- `picture-word`: `Look at the picture, then the word.`
- `word-picture`: `Look at the word, then the picture.`

Show banner, for a right choice and a wrong choice: `Here they are together.`

The ask layout is a prompt on top and the choice cards under it. The prompt is inside a 16 px screen inset and its short side is at least 112 px when the viewport is at least 390 px wide.

- `hear-picture`. The prompt is a button `#prompt` with `data-play="true"`. The canvas draws a flat ink horn in that box: a trapezoid, wide at the left, with a small rectangle at the right. No letters. `aria-label` is `Hear the word`. Visible text is empty. Its writer is `hear`.
- `picture-word`. The prompt is the answer picture on the canvas. It is not a button and not a target.
- `word-picture`. The prompt is `#prompt` with `data-play="true"`. Its visible text is the lowercase word. Its writer is `hear`. It does not choose.

Choice buttons are `#choice-0` onward, in `state().choices` order. `data-play="true"` on ask. `data-id` is the authored id. Picture face: empty visible text, drawing in that box. Print face: `textContent` is the lowercase id, at least 48 px, ink on cream. The writer speaks that word only when the face is `print` and the phase is still `ask`, then calls `choose(id)`. A picture face does not speak.

Two or three choices stack in one column. Four choices are a 2×2. Gap is 16 px under a 700 px width and 24 px at 700 and above. Cards sit at least 16 px in from the screen edge. At 390 px wide, four cards are 171 by at least 140 (`(390 - 32 - 16) / 2 = 171`). Two or three cards use the full inset width and a height of at least 112. Do not drop a card to make them fit. Do not go to six. Do not shrink a card to pretend a 300 px width passes. Hit boxes match the buttons.

Stop is on the trial, menu dwell, no `data-play`, and calls `go("rest")`. Back does the same kind of dwell and calls `go("menu")`.

Show, for 800 ms of `step` time measured from the `step` timestamp that entered show:

- The choice buttons stay where they were. Do not destroy the button the look is on.
- The right card, the one whose id is `prompt`, gets a steady 6 px sun-gold outline `#e2a62e`. That outline is not the gaze ring. Reduced motion does not pulse it and does not skip the 800 ms.
- The prompt area shows the right picture and a `#prompt` button whose visible text is the right lowercase word. That button is `data-play="true"` and its writer is `hear`. This is the pair. It is shown on a miss and on a hit. Tiny Tots see this word only during show, never as a choice during ask.
- `choose` during show does nothing. Choice buttons do not speak during show.
- The card that was wrong ignores a repeat gaze commit until the look moves off it or `releaseLook` runs. The gaze client has no cooldown timer. The writer drops that repeat before speech and before `choose`. `lastChoice` and `said` stay as they were.
- If a dwell on the shown word reaches 650 ms before the beat ends, `hear` speaks it. The beat does not wait. At 800 ms the page advances even if a dwell is unfinished, and an unfinished dwell does not speak.
- A correct printed choice already spoke that word on the same commit. Show does not speak it again for that commit. Staying on the button does not fire a second time.

Advance when `step`'s now is at least show-start plus 800. If the next index is the cap, or the sitting clock is already at the ceiling, set `trial` to the current index plus 1 and `go("rest")`. Otherwise set `trial` to that next index, phase `ask`, clear `lastChoice`, and deal that trial. Do not clear `said`. Do not speak.

`SHOW_MS` is 800. There is no 700 ms beat.

### Rest

Banner: `That was a good sitting.` The card sentence is `We can play again.` An Another button calls `start` for the same band, which lands on subject and deals nothing until a subject is picked. Back returns to the menu and keeps the band highlight. No grade, no miss count, no time, no rank, and none of the words wrong, fail, or no.

**Finished card revised 2026-10-20.** The last trial stays on the page. The prompt and the choices are not targets. The banner still says "That was a good sitting." and the line "We can play again." stays under the banner. Another sits under the cards. Stopping from the subject list, before a trial, still uses the centered rest card.

### Shared hook

`render` runs before any of these return, including while the check flag is on and the frame loop is off.

- `start(band)` sets the band when `band` is `tiny`, `little`, `shop`, or `rush`. An unknown band does nothing. It resets the sitting, sets `said` to null, sets the sitting clock to 0, then `go("subject")`. It does not open the socket.
- `go(screen)` is the only writer of the screen id. Unknown ids do nothing. Allowed ids are `menu`, `subject`, `trial`, and `rest`. Back calls `go("menu")` and clears the dealt trial. The band highlight remains.
- `subject(id)` deals that subject when `level` is set and `id` is in that band's list. It uses a pinned seed if `seed` was called since the last deal, otherwise `playSeed`. It sets `said` to null, sets the sitting clock to 0, sets `trial` to 0, phase `ask`, and `go("trial")`. An unknown id does nothing and is not written into the page.
- `choose(id)` is the only writer of the current answer. An id that is not on the current cards does nothing. A call during `show` does nothing. A call when the screen is not `trial` does nothing. A legal call sets `lastChoice` to that id, appends `{ promptId, direction, correct }` to the sitting history, and sets phase to `show`. `correct` is true only when `id` equals `prompt`. The shown pair is always `prompt`, never the foil.
- `hear()` speaks `state().prompt` when that string is in the current subject's authored list, and does not choose. It does nothing when `prompt` is null or the screen is not `trial`.
- `look(nx, ny)` places a test gaze at window fractions. It does not commit.
- `releaseLook()` drops the test gaze and clears the missed-card ignore.
- `hold(nx, ny)` is a pointer down. A second hold is ignored.
- `release()` is the matching pointer up. It does not fire a button.
- `tap(nx, ny)` runs the writer for the button at that point, with no dwell, and does not leave the pointer down.
- `step(now)` is the only clock.
- `dwell()` returns the ring fill from 0 to 1.
- `eyes()` returns `{ on, live }` and nothing else. No coordinates.
- `centers()` returns window fractions and pixel boxes for every live target.
- `targetsAt(w, h)` returns those boxes for a pretend viewport.
- `state()` returns plain data that `JSON.stringify` can hold. It always includes `level`, `screen`, `banner`, and `muted`. It also includes `subject`, `phase`, `direction`, `prompt`, `choices`, `trial`, `cap`, `said`, and `lastChoice`. It never includes a DOM node, a gaze coordinate, the socket URL, the query string, a score, a wrong count, or a lives field.
- `seed(n)` pins the next deal only. It does not change a sitting that has already started. It is not read from the query.
- `selfCheck()` returns `{ ok, failures }`. Each failure is one sentence that names the case.

`level` is the band id or null before the first `start`. `subject` is the subject id or null. `phase` is `ask`, `show`, or null. `direction` is `hear-picture`, `picture-word`, `word-picture`, or null. `prompt` is the word id or null. `choices` is `{ id, face }` with `face` `"picture"` or `"print"`, or an empty array. `trial` is the 0-based index. `cap` is 8, 12, or 16. `said` is the last word passed toward speech in this sitting, or null. `lastChoice` is the id chosen on this trial, or null.

`start`, a band button, and Let's go share one band writer. A play button, `tap`, and a committed look share that button's writer.

### Flags, storage, sound

Flags are presence checks against the fixed list `selfcheck`, `play`, and `rush`. The value is ignored. The raw search string is never written into text or an attribute. Unknown flags do nothing.

Deep links run only when `selfcheck` is absent. `play` calls `start("tiny")`. `rush` calls `start("rush")`.

No `localStorage` write. No `setItem`, no `removeItem`, no cookie, no `fetch`, no beacon. The storage case may read `localStorage` only to see that this game added no key and that `pop-pals-v1` is unchanged.

Sound other than speech is one oscillator, created after the first pointer press or the first committed gaze action. A choose that enters show plays one sine blip at 660 Hz for about 80 ms. The same blip plays for a right choice and a wrong choice. It is not a buzzer. Master gain is 0.5 unmuted and 0 muted. Mute does not delete buttons. Mute also cancels speech and blocks new speech; while muted, `said` does not change. The self-check does not mute before a speech case. No recorded audio files. `prefers-reduced-motion: reduce` skips decorative motion and still applies the commit and the 800 ms advance.

## Gaze and speech

### Constants

Spelled this way:

- `GAZE_URL` is `ws://127.0.0.1:7081`
- `PLAY_DWELL_MS` is 650
- `MENU_DWELL_MS` is 1300
- `GAZE_FRESH_MS` is 500
- `LEAVE_MS` is 280
- `SHOW_MS` is 800

There is no onset constant. Do not add 200 ms before the ring moves. Do not use a third dwell. A 640 ms look on a play target does not commit. A 660 ms look does. A 600 ms look on a menu button does not commit. A 1300 ms look does.

### Socket

The URL is that literal. It is not built from `location`, a query value, or a message. An `https` page does not open it and does not fall back to another host. On open, the page sends the nine hello bytes 1, 4, 0, 0, 0, 0, 5, 0, 0. Framing is a type byte, a little-endian unsigned 32-bit length, then the payload. Type 1 with at least 56 bytes is a sample. Type 4 with at least 72 bytes is the homography, nine little-endian floats. A non-empty type 4 shorter than 72 bytes is ignored. An empty type 4 clears the homography. A length above 1 MiB resets the buffer. Samples and the homography stay in memory. They are not logged, not put in `state()`, and not written into the page. Reconnect waits 2000 ms, and only if the check flag is absent. The reconnect timer is a function, not a string.

Each gaze sample moves 0.45 of the way toward the new point, as in the horse game. The first sample after `look` or after `releaseLook` snaps to the point, so one `look` plus one `step` is already on that point. The homography is applied before the point is placed. The check does not open the socket. `look` during the check stamps freshness from the `step` clock. `performance.now()` does not decide a dwell during the check.

### Ring and dwell

The ring is 54 px, centered on the pointer, green fill `#2f8a3e` over `rgba(244, 240, 216, 0.45)`, `z-index` above the page, pointer events ignored. It is shown only while the sample is fresher than 500 ms. Fill is `elapsed / dwellMs` from the first moment on the button, clamped from 0 to 1. Off a target, fill is 0, except during the leave grace below, where the fill holds. A stale look does not commit.

Every selectable thing is a real `button`. Hit testing is `elementFromPoint`, then the nearest `button`. A button with `data-play="true"` dwells 650 ms. Every other button dwells 1300 ms. The canvas is not a hit target. There is no blink, no wink, and no drag that commits.

Play targets are the choice cards, the horn, and a printed prompt. Menu targets are the band buttons, Let's go, subject buttons, Stop, Back, Another, mute, and fullscreen. There is no credits control. `#eyes` is not a control.

One pointer at a time. A second contact is ignored until the first ends. While a pointer is down, gaze dwell does not advance and the ring fill stays at 0. A pointer press clears a gaze dwell that was in progress. Releasing the pointer does not itself fire the button. A tap and a finished look call the same writer.

Leave grace. `LEAVE_MS` is not a Faster button. This page has no Faster control.

- Elapsed dwell time increases only while the look is on that same button.
- Moving from one button to another ends the old visit immediately and starts the new one at elapsed 0.
- Slipping off every button holds the visit. Elapsed does not grow while the look is off, and the visit cannot commit while the look is off. If the look returns to the same button before 280 ms, the visit continues from the elapsed time it held. If the look stays off for 280 ms, the visit is discarded, the fill returns to 0, and nothing is spoken and nothing is chosen.
- A look that is still on the button at 640 ms does not commit. A look that leaves at 640 ms does not speak, including after the grace runs out.
- A look that stays on the button until 660 ms commits once. Staying after that does not fire again. The ring holds at full until the visit ends.
- Pointer down and a sample 500 ms old or older cancel the visit immediately. They do not wait 280 ms.

The sitting clock is separate from the dwell. A `step` gap greater than 0 and less than 120000 ms adds to the sitting clock. A gap of 0 or a gap of 120000 ms or more does not. The dwell still reads the timestamp, so one 660 ms step commits. The ceiling is checked at the end of `step` while the screen is `trial`. Crossing it calls `go("rest")` with no fail banner, even in the middle of ask or show. Time on the menu, the subject screen, and rest does not count. `start` and `subject` zero the clock.

### Speech

This is the only game that may call `speechSynthesis`. The utterance text is exactly one string from the authored lists in this spec. It is not a sentence, not a query value, not a banner, and not a picture id by itself unless that picture id is the word that was dwelled. One utterance at a time. A new speak cancels the previous utterance before it starts. Do not queue.

`said` becomes that word when the page commits to speak it. If `speechSynthesis` exists and mute is off, the page then calls `cancel` and `speak` with that one word. If the API is missing, `said` still updates and the page does not throw. Picture choices never call it. Menus never call it. A new trial never calls it.

- A 640 ms look on the horn or on a printed word leaves `said` null when it was null.
- A 660 ms look sets `said` to that exact authored word.
- Leaving at 640 ms cancels the pending commit and does not speak.
- A printed choice speaks that choice's word at the moment the dwell commits, and no other card's word, and then `choose` runs. `choose` does not speak a second time for that commit.
- A picture choice does not change `said`.
- A wrong `choose` does not auto-speak the answer.
- The child may hear the shown correct word only by finishing a dwell on it.

No preview speech. A look that has not committed is silent, including a sweep across every card.

## Generator

The generator is pure. It lives in the page script. The same arguments return the same value. It does not read the clock, the DOM, the query string, the network, or `speechSynthesis`.

### RNG

`makeRng(seed)` takes a uint32. `rngUint(rng)` returns the next uint32. Implement the steps with `Math.imul`. `>>>` is the unsigned shift.

1. `state = seed >>> 0`.
2. `state = (state + 0x6D2B79F5) >>> 0`.
3. `t = imul(state xor (state >>> 15), state or 1)`.
4. `t = t xor (t + imul(t xor (t >>> 7), t or 61))`, with the addition wrapped as a uint32.
5. Return `(t xor (t >>> 14)) >>> 0`.

`rngInt(rng, lo, hi)` is `lo + (rngUint(rng) % (hi - lo + 1))`, both ends included. `rngShuffle(rng, items)` copies the array and runs Fisher-Yates from the last index down to 1: swap `i` with `rngInt(rng, 0, i)`.

Seed 1 must yield these first four uints, in order: `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`.

`playSeed(clockMs, counter)` is `imul( (clockMs >>> 0) xor imul(counter >>> 0, 0x9E3779B9), 0x85EBCA6B ) >>> 0`. `playSeed(1, 1)` is `0x3B230FE8`. The pure generators never call this. The page does, when `seed(n)` was not called: `clockMs` is `Date.now()`, and `counter` starts at 1 and increases by 1 for each deal in the page session. Self-check passes a fixed seed and does not read the clock.

`choiceSeed(seed, index)` is `imul(seed xor (index + 1), 0x9E3779B1) >>> 0`. One trial can be rebuilt without replaying earlier choice shuffles.

### Functions

`flashCap(band)` returns tiny 8, little 12, shop 16, rush 16. Time is still 16.

`flashChoices(band)` returns tiny 2, little 3, shop 4, rush 4.

`flashPlan(band, subject, seed)` returns `{ order, directions }` or `null` if the subject is not in the band. `order` is the prompt id for each trial, length `flashCap(band)`. `directions[i]` is the direction, or `null` when Little Pals must decide at trial time.

`flashTrial(band, subject, plan, index, history, seed)` returns `{ direction, promptId, choices, answerId }` or `null` if `index` is out of range or the plan is null. `history` entries are `{ promptId, direction, correct }` for trials already answered. `choices` is `{ id, face }` with `face` `"picture"` or `"print"`.

| Direction | Prompt | Choice face |
| --- | --- | --- |
| `hear-picture` | word id, spoken only if the page dwells it | picture |
| `picture-word` | picture, not a target | print |
| `word-picture` | printed word, dwell speaks and does not choose | picture |

An unknown subject id returns null and is not copied into any string the page shows.

### Clusters

Clusters exist so Tiny Tots never use a near foil, and so Shopkeeper and Rush can prefer a neighbor. Little Pals do not read clusters.

Tiny animals: dog pet, cat pet, bird sky, fish water, cow farm, duck sky.

Tiny food: milk drink, apple fruit, banana fruit, bread bake, cookie bake, water drink.

Tiny body: eye face, nose face, mouth face, hand limb, foot limb, tummy core.

Those groups are why dog is never beside cat, bird never beside duck, milk never beside water, and eye, nose, and mouth never beside each other, and hand never beside foot. The same rule also keeps apple off banana and bread off cookie.

Shop animals: dog pet, cat pet, cow farm, horse farm, sheep farm, goat farm, pig farm, frog pond, mouse small, duck sky.

Shop food: apple fruit, banana fruit, grape fruit, carrot vegetable, corn vegetable, milk drink, cheese dairy, bread bake, egg egg, soup soup.

Shop household: cup cup, bowl dish, plate dish, spoon utensil, fork utensil, pan cook, pot cook, key key, clock clock, phone phone.

Shop furniture: bed sleep, crib sleep, chair seat, sofa seat, table top, desk top, lamp lamp, shelf shelf, rug rug, door door.

Shop clothes: shoe foot, sock foot, boot foot, coat warm, scarf warm, glove warm, shirt shirt, dress dress, pants pants, hat hat.

Shop vehicles: car road, bus road, truck road, wagon haul, cart haul, bike bike, boat boat, plane plane, train train, sled sled.

Shop outside: tree plant, flower plant, grass plant, leaf plant, sun sky, moon sky, star sky, cloud sky, rock rock, rain rain.

Rush kitchen: ladle utensil, whisk utensil, scoop utensil, grater tool, peeler tool, sieve tool, kettle heat, oven heat, sink sink, tray tray, mixer mixer, mitt mitt.

Rush weather: rain wet, storm wet, puddle wet, hail ice, snow ice, frost ice, cloud sky, fog sky, sun sky, rainbow sky, wind wind, umbrella umbrella.

Rush places: home building, school building, store building, library building, farm building, park land, hill land, garden land, beach water, lake water, bridge bridge, road road.

Rush animals: owl bird, crane bird, fox land, hare land, whale sea, seal sea, dolphin sea, ant bug, bee bug, moth bug, crab crab, toad toad.

Every color id, and every time id, is its own cluster. A word that is the only member of its cluster has no neighbor to prefer.

### Plan

1. `rng = makeRng(seed)`. Shuffle the subject's ids in the authored order from Bands and subjects.
2. Emit each shuffled id twice before the next id. If the cap is longer than two passes, start again at the front of that same shuffled list. Stop at `flashCap`. Tiny uses 4 of 6 words, twice each. Time's four words are used four times through to fill 16.
3. Directions, still using this same rng. Do not reseed between the deck shuffle and the direction shuffle.
   - Tiny: every trial `hear-picture`.
   - Little: leave `directions[i]` null. The trial function decides.
   - Shop: `nRev = floor(16 / 3)`, which is 5. Shuffle indices `0..15` and mark the first 5 as `word-picture`. The other 11 are `picture-word`.
   - Rush: `nRev = 16 / 2`, which is 8, including Time. Exactly 8 are `word-picture`. The other 8 are `picture-word`.

### Trial

`answerId = plan.order[index]`. `promptId` is that same id.

Little direction. Let `missedPrint` be the number of history entries with `direction === "picture-word"` and `correct === false`. Let `heard` be the prompt ids of history entries with `direction === "hear-picture"`. A miss on a heard trial still counts as heard. Use `picture-word` only when all of these hold: the subject is not `colors`, `index % 4 === 3`, `missedPrint < 2`, and `answerId` is in `heard`. Otherwise `hear-picture`. On the doubled plan, index 3 is the second copy of the second word, which was the hear-prompt at index 2, so a normal play can print. Indices 3, 7, and 11 are the only candidates. That is at most 3 of 12, one in four. Two missed print trials stop the rest.

Foils. Candidates start from the subject's ids in authored order.

- Tiny: drop every id in the answer's cluster, including the answer. Take the first remaining id. One foil. A subject always has another cluster.
- Little: take the next 2 authored ids after the answer, wrapping, skipping the answer.
- Shop and Rush: put same-cluster ids first, in authored order, then the other ids in authored order, and take 3. Time has four words and four choices, so the three foils are the other three words.

The choice array before its shuffle is the answer, then those foils, in that order. Shuffle it with `makeRng(choiceSeed(seed, index))`. Every choice gets the face in the direction table. Choices are distinct and include the answer exactly once.

### Seed 31, frozen

Tiny animals order: fish, fish, cow, cow, dog, dog, duck, duck. Every direction `hear-picture`. Choice ids in order:

| Trial | Answer | Choices |
| --- | --- | --- |
| 0 | fish | dog, fish |
| 1 | fish | fish, dog |
| 2 | cow | cow, dog |
| 3 | cow | dog, cow |
| 4 | dog | bird, dog |
| 5 | dog | dog, bird |
| 6 | duck | dog, duck |
| 7 | duck | dog, duck |

The foil cluster is never the answer cluster. Faces are `picture`.

Tiny food order, same seed, for the repeat check: bread, bread, cookie, cookie, milk, milk, water, water. Trial 0 choices are milk, bread. The foil for bread is milk, not cookie.

Tiny body order: hand, hand, foot, foot, eye, eye, tummy, tummy. Trial 0 choices are eye, hand.

Little animals order: horse, horse, fish, fish, bird, bird, dog, dog, cow, cow, duck, duck.

- Empty history, index 3: `hear-picture`, answer fish, choices cow, duck, fish, all `picture`. The word is not heard yet because history is empty, even though index 2 would have been the same word.
- History `[{ promptId: "fish", direction: "hear-picture", correct: true }]`, index 3: `picture-word`, same choice ids, all `print`.
- History of two `{ direction: "picture-word", correct: false }` entries plus a hear of the answer: index 3, index 7, and index 11 stay `hear-picture`.
- If every answer is correct, the twelve directions are hear, hear, hear, picture-word, hear, hear, hear, picture-word, hear, hear, hear, picture-word. The printed answers are fish, dog, and duck. Three of twelve.

Little colors order: green, green, black, black, red, red, white, white, blue, blue, yellow, yellow. Every trial is `hear-picture` and every face is `picture`, even if history has already heard all six ids. Trial 0 choices are black, green, white.

Shop animals order: sheep, sheep, duck, duck, goat, goat, mouse, mouse, frog, frog, cow, cow, pig, pig, cat, cat. Directions by index: picture-word, picture-word, picture-word, picture-word, word-picture, picture-word, word-picture, word-picture, picture-word, picture-word, picture-word, word-picture, picture-word, word-picture, picture-word, picture-word. That is 11 `picture-word` and 5 `word-picture`. Trial 0 is `picture-word`, answer sheep, choices goat, sheep, cow, horse, all `print`. Those foils are the farm cluster.

Rush animals order: crab, crab, dolphin, dolphin, crane, crane, bee, bee, whale, whale, toad, toad, ant, ant, owl, owl. Directions by index: word-picture, picture-word, word-picture, picture-word, word-picture, word-picture, picture-word, word-picture, picture-word, picture-word, word-picture, picture-word, word-picture, word-picture, picture-word, picture-word. That is 8 and 8. Trial 2 is `word-picture`, answer dolphin, choices seal, dolphin, owl, whale, all `picture`. Seal is the sea neighbor. Trial 0 choices are whale, crab, owl, fox.

Time order: evening, evening, night, night, morning, morning, noon, noon, and then that same eight again. Directions by index: word-picture, word-picture, picture-word, picture-word, word-picture, picture-word, word-picture, picture-word, word-picture, picture-word, word-picture, picture-word, word-picture, picture-word, word-picture, picture-word. That is 8 and 8. The four choices on every trial are the four time words.

`flashPlan("tiny", "not-a-subject", 31)` is null.

## Self-check cases

`?selfcheck` is handled before any frame and before any socket. The page sets `checking` and `suppressReconnect`, calls `selfCheck`, creates the `pre`, sets its text with `textContent` to the JSON result, and sets `document.title` to `SELFCHECK_OK` or `SELFCHECK_FAIL`. It then stops. It does not call `requestAnimationFrame`. It does not construct a WebSocket. It does not apply deep links. If a socket already existed, the check closes it and keeps reconnect suppressed. The check drives time only through `step`, from 10000. It does not wait on a frame and it does not read screenshots. The harness opens the file with `?selfcheck` plus one junk flag whose value looks like a URL or a tag. The game cases still pass on that URL.

`engineSelfCheck` returns `{ ok, failures }`. It does not call `Date.now`, `setTimeout`, `setInterval`, `requestAnimationFrame`, `fetch`, `WebSocket`, or `speechSynthesis`. It does not read the DOM. The page runs the family cases, then `engineSelfCheck`, then the page cases, and merges the failures. `ok` is true only when `failures` is empty. The storage assertion runs after the later cases, even though its name is listed with the family.

### Family cases

1. `family-guards`. At the start of the check, `checking` is true, reconnect is suppressed, and the socket is null.
2. `family-no-storage`. After the later cases have run, `localStorage` has no key for this game, `pop-pals-v1` is unchanged from the start of the check, and the page never called `setItem` or `removeItem`.
3. `family-play-dwell`. On Tiny animals, a look on a choice for 640 ms does not commit. A look for 660 ms does. A `tap` on that target runs the same writer.
4. `family-menu-dwell`. A look on Let's go or Back for 600 ms does not change the screen. A look for 1300 ms does.
5. `family-pointer-blocks`. `hold` on a play target, then a 660 ms look, does not commit. `release`, then a fresh 660 ms look, does.
6. `family-stale`. A look that `step` ages past 500 ms leaves `dwell()` at 0 and does not commit.
7. `family-target-size`. `targetsAt(390, 844)` and `targetsAt(1024, 768)` both report a short side of at least 72 for every target. Choice cards on those viewports are at least 112. `targetsAt(300, 600)` reports at least one short side under 72 instead of shrinking a target.
8. `family-eyes`. `eyes()` has only the boolean keys `on` and `live`. A fresh test look sets `live` true with the socket still closed. The Eyes chip is shown for that look.
9. `family-mute`. The mute button's visible text is empty, it contains two inline speaker SVGs, and the words Mute and Sound are not visible. `tap` sets `state().muted`. A second `tap` clears it.
10. `family-query-inert`. The junk flag's value is not in the document text or in any attribute, the screen is still the menu, and no socket was opened.
11. `family-plain-state`. `state()` can be turned into JSON and contains `level`, `screen`, `banner`, and `muted`.

### Engine cases

`engine-rng`. `makeRng(1)` yields `0xA087EAF3`, `0x00B349C9`, `0x8706C4EB`, `0xFB2627FD`. `playSeed(1, 1)` is `0x3B230FE8`. `rngInt` on a fresh `makeRng(1)` stays inside `0..3`.

`engine-tiny`. Seed 31, Tiny animals, matches the frozen order, the eight choice lists, length 2, every direction `hear-picture`, every face `picture`, every id in the six animals, exactly one answer, and the foil cluster differs from the answer cluster. Each id that appears does so twice before a different id appears.

`engine-little`. Seed 31, Little animals, matches the empty-history, heard-history, and two-miss cases above, and the correct-play directions. Little colors never returns `picture-word` and never returns face `print`.

`engine-shop`. Seed 31, Shop animals, four choices, 11 `picture-word`, 5 `word-picture`, frozen order and trial 0 choices. Foils are from that subject. Exactly one choice equals `answerId`.

`engine-rush`. Seed 31, Rush animals, 8 `word-picture` and 8 `picture-word`, frozen order. Not all one direction. Time is length 16, the frozen order, 8 and 8, and the words are only morning, noon, evening, and night.

`engine-unknown`. `flashPlan("tiny", "not-a-subject", 31)` is null.

Every trial in those plans has distinct choices, length `flashChoices`, and exactly one `answerId`.

### Page cases

1. `tiny-two-pictures`. `seed(31)`, `start("tiny")`, and `subject("animals")` set `choices` length 2, `direction` `hear-picture`, and `prompt` `fish`. No choice has face `print`. No choice has visible text. No `+`, `−`, or `=` is on the trial. `said` is null. The horn does not speak on deal.
2. `speech-on-dwell`. A 640 ms look on `#prompt` leaves `said` null. A fresh 660 ms look sets `said` to `fish` and does not change `lastChoice`. A look that leaves at 640, then stays away for at least 280 ms, leaves `said` null.
3. `picture-silent`. `choose` of a picture id does not change `said`.
4. `printed-word-speaks`. `seed(31)`, `start("shop")`, `subject("animals")`. Trial 0 is `picture-word`. A committed look on one printed choice sets `said` to that choice's word and not to any other card's word.
5. `miss-no-tally`. A wrong `choose` leaves `state()` without a score, wrongs, or lives field. The banner is `Here they are together.` and does not contain fail or wrong. `phase` is `show`. `prompt` is still the answer, not the foil. The outlined card is the answer.
6. `miss-cooldown`. After that wrong gaze commit, a second gaze commit on the foil, without `releaseLook` and without a move off it, does not speak again and does not change `lastChoice`.
7. `show-then-next`. From the `step` that entered show, a step 799 ms later does not change `trial`. A step that reaches 800 ms increases `trial`. On the last trial, that step sets `screen` to `rest` and `trial` to `cap`.
8. `repeat-before-new`. With seed 31, Tiny animals prompts are fish twice, then cow twice, then dog twice, then duck twice.
9. `little-print-budget`. Seed 31. Print trials are at most one in four, only for a word already heard, and only at index 3, 7, or 11. After two missed print trials, `direction` stays `hear-picture`. Colors never print.
10. `shop-four`. Four choices. Eleven trials are `picture-word` and five are `word-picture`. The default is `picture-word`.
11. `rush-half`. Seed 31, Rush animals. The reverse count is 8 of 16. Time is also 8 of 16. It is not all one direction.
12. `colors-not-print`. Little colors has six picture ids and no printed color word on the trial. A Shop colors `picture-word` trial may print the color id. Orange there is the color.
13. `cap-rest`. The eighth Tiny show calls `go("rest")`. The rest card has no grade and its text does not contain fail or wrong. Separately, after `subject("animals")` at step time T, two gaps of 119000 ms leave the screen on `trial`, and the next gap of 2000 ms calls `go("rest")`. A single gap of 240000 ms does not, because it is not under 120 seconds. The rest banner does not say fail.
14. `subject-reject`. `subject("not-a-subject")` does not change the screen and does not insert that string into the page.
15. `no-remote-art`. The document has no image URL, no font URL, and no script URL. Every id in Bands and subjects has a painter. Nothing named Mulberry is in the file.

## README paragraph

Insert this bullet in the Play list, after Saddle Up! and before any later game:

- [Flash Cards](https://calledtoconstruct.github.io/games/flash-cards/)

Insert this section after the Saddle Up! section and before Add a game.

## Flash Cards

A word and picture game for ages 2 and up. The child hears a word by looking at it, then looks at the matching picture. Older bands see more printed words. A sweep across the cards stays quiet. Speech is the browser's own voice, and it starts only after a finished look at a word. A miss shows that picture and that word together. The page does not keep a score, and it does not say the word wrong.

Tiny Tots hear one word and pick between two pictures. There is no printed choice. Little Pals pick among three pictures, and a few trials show the picture first and three words, only for a word already heard. Shopkeeper uses four cards. The picture comes first and the words are printed, and about one trial in three goes the other way. Rush uses four cards too, and about half the trials start from the printed word.

Open `flash-cards/index.html` in a browser, or play [Flash Cards](https://calledtoconstruct.github.io/games/flash-cards/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at a card until the ring fills. Subject buttons, Stop, and Back to Menu take twice as long, so reading one does not press it. Mouse and touch work in the same sitting. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Key Decisions

**Tiny Tots hears a word and picks a picture.** Those players do not read, so a printed choice would not be a real choice. The direction is `hear-picture`: the prompt is one authored word, and the two choices are pictures with no letters. That is the same match as the older bands, in the early direction. It is not picture-then-word. The gaze note that puts two word cards under a picture for Tiny Tots and Little Pals is rejected. It would fail `tiny-two-pictures`, and it would print a choice the youngest band cannot read. The horn is how they ask for the word. Shopkeeper and Rush keep the mature shape the other way around: the usual trial is `picture-word`, and the reverse `word-picture` is about one in three for Shopkeeper and half for Rush, pinned by the seed rather than rolled again at runtime.

**Speech waits for a finished dwell.** The family rule is narrower than a teaching note that speaks when the card appears. Speech when a trial appears is rejected, including the horn. Speech of a word the child did not dwell is rejected, including speaking the answer after a miss. Picture choices never speak. The utterance is one authored word, one at a time, cancelled if the look leaves before 650 ms. A sweep stays quiet because quiet is what keeps the right word from being read aloud on the way to it.

**The clock is the shared clock.** Play is 650 ms and menu is 1300 ms, measured across the whole dwell. The ring fills from the first moment on the button. The 200 ms onset is rejected. It would be a second timer, and 640 ms would no longer be a clean miss. `LEAVE_MS` is 280. It holds a visit that slips off a button for a moment. It does not add a Faster control, it does not commit while the look is off, and it does not let a 640 ms look speak. 660 ms on the button commits. Menu buttons take twice as long so reading one does not press it.

**A miss shows the pair and moves on.** A wrong card does not add a score, a wrongs field, or a life. The banner is `Here they are together.` It does not say wrong or fail. The right picture and the right word are shown, and the right card is outlined, for 800 ms. Then the next trial deals, or rest, if the cap is done. The age line that hides the answer and stays, and the gaze line that stays on the trial and then speaks the answer by itself, are rejected. The child can still hear the right word by looking at it during the beat. If they do not, the beat ends anyway. The missed card cannot fire again until the look leaves or `releaseLook` runs, and that guard is in the writer, not a new gaze timer.

**The word lists are the ones in this spec.** Each word has a canvas drawing here. Alternate lists that swap in belt, sled on Little Pals, hen, today, and tomorrow are rejected. Today and tomorrow are not one object, so they have no drawing. Time is morning, noon, evening, and night. The plan length is still `flashCap`, so those four words repeat until 16 trials. A Time cap of 8 is rejected because it would make the Rush cap depend on the subject and it would no longer match the half-and-half plan of 8 and 8. Shop colors are the ten disks, not the little six, because this band may print color names and the deck is 10. Little colors stay six disks and never print.

**Little Pals print is a budget, not a mode.** At most three of twelve trials, only index 3, 7, or 11, only `picture-word`, only a word already heard as `hear-picture`, and never colors. Two wrong print trials end the budget. The doubled schedule is what makes index 3 printable in real play: the child just heard that same word at index 2. Shopkeeper's five reverses and Rush's eight reverses are chosen by shuffling trial indexes with the plan rng after the deck shuffle, so a check can rebuild them from seed 31.

**Pictures are drawings in the page.** No Mulberry file, no symbol download, no remote image, no font URL, no second script. A missing painter fails the check. Color chips are disks, and the white disk has a rim so it does not disappear on a cream card.

**One page, no memory.** `window.flashCards` is the whole API. `?selfcheck` runs synchronously and then stops, with no frame and no socket. The page does not write `localStorage`. Pins go through `seed`, not through the query. A query value cannot become a subject id, a card, or an utterance.

## Out of scope

- The other four games, and any edit to Pop Pals, Sugar Counter, Sun Garden, or Saddle Up!.
- A shared gaze file, a bundler, a second script, or an asset folder.
- `localStorage`, cookies, accounts, `fetch`, and beacons.
- Downloaded symbols, Mulberry files, remote images, remote fonts, and a credits button or credits file.
- Speech on appear, speech of a picture that was not dwelled, speech of a sentence, preview speech, and any `speechSynthesis` use outside this game.
- An onset timer, a third dwell, blink, wink, and a drag that commits.
- A sixth choice card, a keypad, a typed name, and recorded audio.
- A score, a streak, a life, a rank, a visible clock, a buzzer, and a banner that says wrong or fail.
- Picture-then-word as the Tiny Tots trial, printed Little Pals color words, and the words today and tomorrow.
- A Time sitting of 8 trials. The cap is 16.
- Replaying a missed trial in place. The pair is shown, then the sitting moves on.
- Magnifying a card under the look, or a second ring on the card. The only ring is the 54 px pointer ring.

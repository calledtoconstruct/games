# Picture-and-word flash cards on gaze dwell

Date of research: 2026-10-18. One local browser game. Mouse, touch, and gaze commit the same way: about 650 ms on a play card, about 1300 ms on a menu. Targets stay large. Speech is on-device `speechSynthesis` in Chromium. No account, no picture network, no recorded-audio budget.

The fair game is not one task with a coin-flip reverse. Hearing a word and pointing to a picture is early receptive vocabulary. Matching a printed word is later. Speaking on a finished dwell, and never on a glance, is what keeps a pass across the row silent.

## Evidence

### Hearing a word and finding the picture is the early task

The Peabody Picture Vocabulary Test — Revised shows four drawings. The examiner says one word. The child points. No reading. The [NLS documentation](https://www.nlsinfo.org/content/cohorts/nlsy79-children/topical-guide/assessments/peabody-picture-vocabulary-test-revised) says the PPVT-R was built for ages 2½ to 40, with 175 plates of four pictures, and that the youngest children scored worst, which the NLS authors attribute to unfamiliarity with a test rather than to weaker vocabulary. A [Riot IQ summary dated 2026-08-16](https://www.riotiq.com/articles/specific-iq-tests-and-formats/what-is-the-peabody-picture-vocabulary-test) describes the current PPVT-5 the same way: four pictures, a spoken word, ages 2 years 6 months through adulthood, about 10 to 15 minutes, and no reading or speaking required. That page is a commercial explainer, not the manual. The four-picture spoken-word format itself is also in the NLS primary documentation.

That task is receptive. It is not “see a picture, say the name.” A [Pabau clinic post dated 2026-08-12](https://pabau.com/blog/expressive-one-word-picture-vocabulary-test/) describes the Expressive One-Word Picture Vocabulary Test as naming a picture, from about age 2, and says it does not measure receptive vocabulary. That is one vendor-adjacent source, not the test manual. It is enough to keep the labels apart: this game cannot score a spoken name, because the only selection is dwell. Picture-then-printed-word is a print match. Printed-word-then-picture, if the word is not read aloud, is a reading check. Neither one is the toddler receptive task.

Calling word-then-picture the “mature reverse” is right only when the prompt is print. Spoken-word then picture is the early direction. [Goldfield, Gencarella, and Fornari](https://pmc.ncbi.nlm.nih.gov/articles/PMC5421562/) describe intermodal preferential looking as two images, a spoken label, and gaze toward the match, built for infants and toddlers and extended by them to noun, verb, and adjective comprehension at ages 3–6. Nouns beat verbs and adjectives. Looking scores tracked PPVT-4 more closely than a longest-look score did.

### How many choices

Two pictures is the field used when the behavior is just looking ([Goldfield et al.](https://pmc.ncbi.nlm.nih.gov/articles/PMC5421562/)). Four pictures is the field a pointing test uses from the late twos ([NLS](https://www.nlsinfo.org/content/cohorts/nlsy79-children/topical-guide/assessments/peabody-picture-vocabulary-test-revised); [Riot IQ on PPVT-5](https://www.riotiq.com/articles/specific-iq-tests-and-formats/what-is-the-peabody-picture-vocabulary-test)). Four is a lot for gaze. [Borgestig and colleagues](https://pmc.ncbi.nlm.nih.gov/articles/PMC4867850/) followed 10 children, ages 1–15, who were new to gaze selection and could not speak. Time on task improved after 5 months. Accuracy at hitting targets improved after 15–20 months. A two-by-two of large cards is plausible for school age. Six small cards is not, under the rule that targets stay large.

Chance is 1 in 2 with two cards and 1 in 4 with four. This is a game, not a test, and a wrong card is not a harsh fail, so a high chance rate is acceptable if each word is repeated and the foil is obvious. Three choices is the step between those two published fields. No fetched study set 3 as a standard.

### Dwell, glances, and when speech helps or spoils

A fixation is short. The [Eyegaze Edge vendor note](https://eyegaze.com/wp-content/uploads/How-Does-it-Work-1.pdf) says a fixation usually lasts about one-fifth to one-third of a second, a saccade across a screen is on the order of a sixteenth of a second, and users report less fatigue with dwell from about one-fifth to two-thirds of a second. The same note says holding still on a small bright target is what tires people, and that extraocular muscles are not what fatigue; eyelids and busy screens are. A 650 ms play dwell sits at the slow end of that short-dwell band, past a normal passing fixation, so a glance does not commit. A 1300 ms menu dwell is outside that band on purpose.

[Forbes AAC, 11 June 2026](https://forbesaac.zendesk.com/hc/en-us/articles/36252152235149-How-do-I-adjust-the-dwell-time), states the tradeoff directly: too short and selections are unintentional; too long and the person is slow and tires. That is a support article, not a trial.

One faster dwell is not a reason to speak early. [Walker and Wegner, ATIA volume 15, winter 2021](https://www.atia.org/wp-content/uploads/2021/03/V15_WalkerWegner.pdf), report a single child for whom 300 ms was the dwell that worked in phase 2. That is one learner who could not hold longer, not a default for a word list.

Preview speech is a different feature from commit speech. The [Tobii Dynavox CVI accommodations guide, marked ©2025](https://download.mytobiidynavox.com/Diagnosis%20Kits/CVI/TD_CVI_AacAccommodations.pdf), describes auditory feedback access that “reads button label before selecting,” including for eye gaze, and a separate click when a page changes. Pre-select reading is for people who need to hear a button to know what it is. On a matching trial it names every card the eyes rest on, including the answer, before a choice exists. Commit speech does not do that.

[Sowers (defense 23 May 2022)](https://etda.libraries.psu.edu/catalog/20385djs6974) compared visual feedback during gaze selection in adults without disabilities. Highlighting slowed latency to select. Pointer and magnify did not differ from no feedback on latency, and people did not prefer the condition they were fastest in. The abstract’s conclusion is that intermediary visual feedback, often assumed necessary, can slow selection. A thin dwell ring is enough. Do not magnify the card under the gaze.

No fetched study tested whether hearing foil words spoils a picture-word game. The split below is an inference from the preview-versus-commit distinction in the Tobii guide, plus the fixation lengths in the Eyegaze note.

### What the words should be, and how long a sitting is

[Frank, Braginsky, Yurovsky, and Marchman, chapter 12 of the Wordbank book](https://langcog.github.io/wordbank-book/categories-semantic.html), use the noun-side categories shared by CDI forms: Animals, Body Parts, Clothing, Food and Drink, Furniture and Rooms, Games and Routines, Household, Outside, People, Places, Sounds, Time Words, Toys, Vehicles. Across languages, sounds, games and routines, body parts, toys, animals, and vehicles show up early. Places and time words are systematically late. They cite work that time words are still hard through about age five. Color words are partial for a long time: children learn some colors and overextend them ([Wagner, Dobkins, and Barner 2013](https://langcog.github.io/wordbank-book/categories-semantic.html), as summarized in that chapter). In their mapped set the usual order is red, then yellow, blue, and green, then black and white. Animal and body-part learning is more consistent across languages than color or time. The English CDI itself is for toddlers (Words and Gestures roughly 8–18 months, Words and Sentences 16–30 months); the category order is still the right spine for later bands, with harder categories added, not a random theme list.

[North Carolina DPI’s pre-K vocabulary page](https://www.dpi.nc.gov/students-families/parents-corner/literacy-home-digital-childrens-reading-initiative/pre-kindergarten/vocabulary-pre-k) is about talk and categories during real routines, not a card count. It does describe the same everyday sets: food, kitchen, clothes, animals.

Attention minutes on the open web are clinic claims, not trials. [Expressable, 13 January 2025](https://www.expressable.com/learning-center/babies-and-toddlers/guide-to-understanding-your-toddlers-attention-span), an SLP practice, says about 5–8 minutes on a structured task at 25–36 months, and says they would not ask a toddler to watch a screen for 30 minutes. [CNLD, 28 April 2022](https://www.cnld.org/how-long-should-a-childs-attention-span-be/), lists 4–6 minutes at age 2, 6–8 at 3, 8–12 at 4, 12–18 at 5–6, 16–24 at 7–8, and 20–30 at 9–10, with no study named in the text. [Brain Balance](https://www.brainbalancecenters.com/blog/normal-attention-span-expectations-by-age) repeats the same “two to three minutes per year of age” table. Treat those as ceilings. Gaze adds eyelid fatigue and a plain field ([Eyegaze note](https://eyegaze.com/wp-content/uploads/How-Does-it-Work-1.pdf)), so a sitting should end inside the lower half of the clinic range. No fetched intervention trial gave a required number of new words per session. Repeating a short list is the inference.

### Pictures that can ship

Draw the cards. The list below is a few dozen concrete nouns. Original canvas drawings need no font, no download, and no license, and they match the family’s existing canvas art. Keep them flat, one object, high contrast, large, and free of letters. The word is a separate text card.

[Mulberry Symbols](https://mulberrysymbols.org/) are the legal option if a later build must match the sibling app. The site states Creative Commons BY-SA 4.0, commercial use allowed, no charge for the symbols themselves, and derivatives under the same license. [Open Symbols](https://www.opensymbols.org/repositories/mulberry) lists Steve Lee as primary author and 3,116 symbols. The site’s own credit line is: “Mulberry Symbols by Steve Lee are licenced under the Creative Commons Attribution-ShareAlike 4.0 License. See https://mulberrysymbols.org for details.” Garry Paxton originated the set in 2008 and assigned the copyright to Steve Lee in 2018; the license sentence they ask distributors to use names Lee. The [CC BY-SA 4.0 deed](https://creativecommons.org/licenses/by-sa/4.0/deed.en) requires attribution, a license link, a note if you changed anything, and ShareAlike on adaptations. Unmodified SVGs displayed beside original game code are the safe shape. Redrawing Mulberry into the canvas and dropping the credit is not. Do not recolor a symbol inside the single HTML file and treat the whole file as proprietary. If Mulberry is used, keep those SVGs unmodified, put `LICENSE-MULBERRY` and that sentence in the repo, and put the same sentence on a large credits control. Do not fetch symbols at runtime.

[OpenMoji 17.0.0](https://openmoji.org/) is CC BY-SA 4.0 for the graphics (the site says 4,495 emojis). The [GitHub README](https://github.com/hfg-gmuend/openmoji) says graphics are CC BY-SA 4.0, code is LGPL-3.0, and asks for: “All emojis designed by OpenMoji – the open-source emoji and icon project. License: CC BY-SA 4.0.” SVG paths can be inlined without a font. ShareAlike has the same trap as Mulberry, and the pictures are emoji, not communication symbols. Do not use OpenMoji as the card art. Do not ship the font.

Twemoji graphics are still offered as CC BY 4.0 on the pages read for this note. [twitter/twemoji `LICENSE-GRAPHICS`](https://github.com/twitter/twemoji/blob/master/LICENSE-GRAPHICS) is the CC BY 4.0 legal text. That repo’s README still says the graphics are CC BY 4.0 and that a README, About, or footer credit is enough; a mention in the HTML or JS source is also something they accept. The page describes Twemoji v14.0.3. The maintained fork [jdecked/twemoji](https://github.com/jdecked/twemoji) describes v17.0.2 and the same CC BY 4.0 attribution rule. No ShareAlike. A few inlined SVGs plus “Twemoji graphics by Twitter, Inc. and other contributors, licensed CC BY 4.0” and a link to the repo used would be legal. They are still the wrong pictures for spoon, sock, or crib, and they will not match canvas art. Do not make them the set. Do not load an emoji font.

Do not scrape image search. Do not use commercial clip art.

## Spec

This section is the recommendation. Choice counts and the early-versus-late direction are tied to the evidence above. Exact word lists, reverse ratios, and trial counts are design choices inferred from that evidence plus the product rules (650 ms play, 1300 ms menus, large targets, no harsh fail, local speech only).

### Bands

One subject per sitting. The player picks it from a large menu (1300 ms). No scores are stored. Each target is scheduled twice before the sitting looks for a new word. Foils come from the same sitting’s list. Tiny Tots foils should look unlike the target (fish against cow, not dog against cat). From Shopkeeper up, foils may be the nearer neighbors.

| Band | On-screen choices | Direction | Deck in one sitting | Stop |
| --- | --- | --- | --- | --- |
| Tiny Tots, about 2–3 | 2 | Spoken word, then picture. No printed-word choices. | 6 words, one subject | About 8 trials, or 4 minutes |
| Little Pals, about 4–5 | 3 | Same, spoken word then picture. At most 1 in 4 trials may be picture then printed word, and only for a word already heard this sitting. After two missed print trials, stop offering print. | 8 words, or 6 if the subject is colors | About 12 trials, or 8 minutes |
| Shopkeeper, about 6–8 | 4 | Default is picture, then printed word. About 1 in 3 trials, at random, show the printed word and ask for the picture. | 10 words | About 12–16 trials, or 12 minutes |
| Rush, about 9+ | 4 | About half the trials reversed, chosen at random: printed word prompt, picture choices. The other half stay picture then printed word. | 12 words | About 16 trials, or 15 minutes |

Do not go to six choices. Four is already the PPVT field, and only with cards that stay large.

Subjects, in order. Stay inside one subject for the sitting.

- Tiny Tots: Animals (dog, cat, bird, fish, cow, duck). Food (milk, apple, banana, bread, cookie, water). A later sitting may use body parts (eye, nose, mouth, hand, foot, tummy). No colors. No time words. No places.
- Little Pals: those, plus clothing (hat, shoe, sock, coat, shirt, pants), toys (ball, car, doll, block, book, train), vehicles (car, bus, bike, boat, plane, train). Colors only as a spoken-word, picture-chip subject: red, yellow, blue, green, black, white. Not as printed words.
- Shopkeeper: household (cup, spoon, bowl, bed, chair, door, key, clock, phone, lamp), furniture and rooms, outside things, more clothing, vehicles, and less obvious animals (sheep, goat, frog, horse, pig, mouse). Color names may now be printed words.
- Rush: kitchen tools, weather, places, and less common animals (owl, fox, whale, crab, ant, bee), plus a short time set if wanted (morning, night, today, tomorrow). Time is still the hard category through age five in the Wordbank synthesis, so it is a Rush subject, not an earlier one. Skip games-and-routines phrases; they do not make a single picture.

A wrong card is not a fail state. There is no buzzer, no lost life, and no account. The corrective model is one calm replay of the right pair, described under speech.

### When speech fires

Speech is off on gaze enter. The dwell timer is the only gate. If the eyes leave before 650 ms, cancel the timer and cancel any queued utterance. A sweep across the row stays silent. One utterance at a time. A new commit replaces the old one. Use `speechSynthesis` only. Do not ship recorded clips.

- Picture choice: never speak.
- Printed word choice: speak that word at the moment the 650 ms dwell commits. Do not speak the words that were not chosen. That meets “a dwelled word is read aloud” without previewing the list.
- Spoken prompt (Tiny Tots and Little Pals): say the target once when the trial appears. That is the item, the way an examiner says the PPVT word. It is not a glance. Say it again only if they dwell the prompt card. Dwelling the prompt does not select a choice.
- Printed prompt (Shopkeeper reverse and Rush reverse): do not speak when it appears. Speak only if they dwell the prompt, as a hint they asked for. Dwelling it does not select a picture.
- After a correct commit: the word already spoken is the confirmation. Show picture and word together briefly. No extra praise track.
- After a wrong commit: the committed word has already been spoken if it was a word card. Then show the correct picture and the correct word together and speak that correct word once. Then the next trial. The correction is after the try, so it does not give the answer away during the search.
- Menus, including subject, stop, and credits: 1300 ms, no speech.

Do not turn on AAC-style “read the label before selecting” for choice cards. That is the feature that speaks a whole list. A simple dwell ring is enough visual warning. Do not magnify the card under the gaze ([Sowers](https://etda.libraries.psu.edu/catalog/20385djs6974)).

### Pictures

Original canvas drawings for every card. No font file. No photo. No network image.

Mulberry only if the pictures must be the sibling symbol set: unmodified SVGs, CC BY-SA 4.0, the Steve Lee sentence above, in the repo and on the credits control. Not mixed into a redraw. OpenMoji and Twemoji are licensable and the wrong art; leave them out.

## Gaps

The [Wandin et al. full text](https://www.tandfonline.com/doi/full/10.1080/17483107.2021.1967469) was blocked, so a snippet about a 0.5 s dwell was not used. The Wagner, Jergens, and Barner 2018 PDF was blocked; color claims here are from the Wordbank chapter’s account of that research, which was read. The Sandhofer and Smith adjective PDF could not be fetched. Attention minutes are clinic pages without a named study. No source found tested speech-on-preview inside a children’s matching game. PPVT-5 age bounds and the EOWPVT description are secondary pages, cross-checked against the NLS PPVT-R documentation where they overlap. Twemoji was still presented as CC BY 4.0 on the repo pages fetched; this note did not audit every later commit.

sources_reviewed: 132

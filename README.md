# Games

Browser games we can back up and share. Each game lives in its own folder and is one HTML file, so it runs with no install.

## Pop Pals

A touchscreen whack-a-mole for ages 2 and up. Pick a place (safari, ocean, pets, or forest), tap the animals that live there, and hear their sounds.

Open `pop-pals/index.html` in a browser, or play it on GitHub Pages once that site is published.

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an animal until the ring closes and it pops. Look away and the look is cancelled. Menu buttons take twice as long, so reading one does not press it. Touch still works. An https page will not open that socket. Fullscreen the page on the panel that was calibrated. Every level runs about 15% slower than the first version, and the round clock is longer by the same amount.

## Add a game

Make a new folder, put an `index.html` in it, and push.

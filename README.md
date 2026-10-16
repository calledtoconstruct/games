# Games

Browser games we can back up and share. Each game lives in its own folder and is one HTML file, so it runs with no install.

## Pop Pals

A touchscreen whack-a-mole for ages 2 and up. Pick a place (safari, ocean, pets, or forest), tap the animals that live there, and hear their sounds.

Open `pop-pals/index.html` in a browser, or play it on GitHub Pages once that site is published.

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an animal until the ring closes and it pops. Look away and the look is cancelled. Menu buttons take twice as long, so reading one does not press it. Touch still works. An https page will not open that socket. Fullscreen the page on the panel that was calibrated. Every level runs about 15% slower than the first version, and the round clock is longer by the same amount.

## Sugar Counter

A bakery and ice cream counter for ages 2 and up. Pick a level, then decorate. Tiny Tots get donuts and cupcakes, with no clock. Little Pals add cakes and picture orders, still with no clock. Each visit shuffles the order deck, and the same card does not come back as the very next one when the deck starts over. Shopkeeper adds scooped cones and soft serve, plus a four-minute wait that ends with "Take your time" and does not send the customer away. Rush packs a box of two or three treats. The customer stays calm, then patient, then a little worried, and leaves kindly if the box is not finished. A finished box brings the next customer. A short press leaves one icing splat, one scoop, or one swirl. A longer drag draws a stripe of splats. Sprinkles scatter in a little cloud. Drag a scoop off the cone to put it back. Hold a soft-serve press to make the swirl taller. When a customer leaves, the treat stays on the counter. Item ready sits beside the treat. Back to Menu returns to the level picker and keeps the level you picked. On Rush the customer starts happy and grows impatient as the clock runs down.

Open `sugar-counter/index.html` in a browser.

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at the donut until the ring fills, then move to draw. Looking away releases the icing. Tray buttons, Back to Menu, Item ready, Mute, and Fullscreen take twice as long, so a glance does not press them. Mouse and touch work in the same round. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Sun Garden

A garden for ages 2 and up. Flowers is a flower garden. Market grows food for the town. Tiny Tots water two big flowers while the sun moves. Little Pals can park the sun. Shopkeeper adds trees and a shade cloth that lasts half a day. Rush uses the sun's trip across the sky as the clock. A cart comes for every ripe crop and leaves anything still growing. Food the town asked for cheers them up. Other ripe food makes them tired of that food. Banana and coconut trees stay standing: the cart takes the fruit, and only every second or third crop does it take the whole tree. On Rush the town can ask for two foods at once, or more when every one of them is a fast grower. The town only asks for a food that is growing in the garden. A face on the market card shows how the town feels: enough of the food they want makes them happy, and the same unwanted food twice makes them tired of it. Each visit shuffles which plant is in which plot. After a harvest of any other crop, or when a tree is finally taken, that plot sprouts a different plant. The town only asks for a plant that is still in the garden.

Open `sun-garden/index.html` in a browser.

Mouse, touch, and gaze do the same work. Holding Water puts a can on the pointer and squirts the bed under it. The badge shows how grown that plant is and a Water bar. Each kind drinks at its own pace, faster in full sun and slower when the sun is far away. A dry plant wilts, and it stays wilted, a little less so, until the water comes back. Sun-loving plants still grow faster in direct sun. Each plot holds several plants of one kind. They sprout, lean a little toward the sun, and ripen on their own clocks. Palms grow far above the vines. Tomatoes run along the ground. Left alone they creep. Water and the right light make a plant finish in more than one day. Too much sun for that kind wilts it. Drag the sun along its arc to park the light, and let go to let the day continue. Look at a plant or the sun until the ring fills, then keep looking to hold. Looking away lets go. Menu buttons take twice as long. Fullscreen the page on the panel that was calibrated. An https page will not open the gaze socket.

## Add a game

Make a new folder, put an `index.html` in it, and push.

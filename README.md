# Games

Browser games we can back up and share. Each game lives in its own folder and is one HTML file, so it runs with no install.

Joseph Woolley made these games. The license is [MIT](LICENSE).

## Play

- [Pop Pals](https://calledtoconstruct.github.io/games/pop-pals/)
- [Sugar Counter](https://calledtoconstruct.github.io/games/sugar-counter/)
- [Sun Garden](https://calledtoconstruct.github.io/games/sun-garden/)
- [Saddle Up!](https://calledtoconstruct.github.io/games/saddle-up/)
- [Sudoku](https://calledtoconstruct.github.io/games/sudoku/)
- [Math Practice](https://calledtoconstruct.github.io/games/math-practice/)
- [Word Search](https://calledtoconstruct.github.io/games/word-search/)
- [Flash Cards](https://calledtoconstruct.github.io/games/flash-cards/)
- [Maze](https://calledtoconstruct.github.io/games/maze/)

## Pop Pals

A touchscreen whack-a-mole for ages 2 and up. Pick a place (safari, ocean, pets, or forest), tap the animals that live there, and hear their sounds. The speeds are Burrow, Meadow, Explorer, Ranger, and Ranger Rush.

Open `pop-pals/index.html` in a browser, or play [Pop Pals](https://calledtoconstruct.github.io/games/pop-pals/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an animal until the ring closes and it pops. Look away and the look is cancelled. Menu buttons take twice as long, so reading one does not press it. Touch still works. An https page will not open that socket. Fullscreen the page on the panel that was calibrated. Every level runs about 15% slower than the first version, and the round clock is longer by the same amount.

## Sugar Counter

A bakery and ice cream counter for ages 2 and up. Pick a level, then decorate. Sprinkle gets donuts and cupcakes, with no clock. Birthday adds cakes and picture orders, still with no clock. Each visit shuffles the order deck, and the same card does not come back as the very next one when the deck starts over. Scooper adds scooped cones and soft serve, plus a four-minute wait that ends with "Take your time" and does not send the customer away. Lunch Rush packs a box of two or three treats. The customer stays calm, then patient, then a little worried, and leaves kindly if the box is not finished. A finished box brings the next customer. A short press leaves one icing splat, one scoop, or one swirl. A longer drag draws a stripe of splats. Sprinkles scatter in a little cloud. Drag a scoop off the cone to put it back. Hold a soft-serve press to make the swirl taller. When a customer leaves, the treat stays on the counter. Item ready sits beside the treat. Back to Menu returns to the level picker and keeps the level you picked. On Lunch Rush the customer starts happy and grows impatient as the clock runs down.

Open `sugar-counter/index.html` in a browser, or play [Sugar Counter](https://calledtoconstruct.github.io/games/sugar-counter/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at the donut until the ring fills, then move to draw. Looking away releases the icing. Tray buttons, Back to Menu, Item ready, Mute, and Fullscreen take twice as long, so a glance does not press them. Mouse and touch work in the same round. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Sun Garden

A garden for ages 2 and up. Flowers is a flower garden. Market grows food for the town. Sprout waters two big flowers while the sun moves. Sunbeam can park the sun. Orchard adds trees and a shade cloth that lasts half a day. Market Day uses the sun's trip across the sky as the clock. A cart comes for every ripe crop and leaves anything still growing. Food the town asked for cheers them up. Other ripe food makes them tired of that food. Banana and coconut trees stay standing: the cart takes the fruit, and only every second or third crop does it take the whole tree. On Market Day the town can ask for two foods at once, or more when every one of them is a fast grower. The town only asks for a food that is growing in the garden. A face on the market card shows how the town feels: enough of the food they want makes them happy, and the same unwanted food twice makes them tired of it. Each visit shuffles which plant is in which plot. After a harvest of any other crop, or when a tree is finally taken, that plot sprouts a different plant. The town only asks for a plant that is still in the garden.

Open `sun-garden/index.html` in a browser, or play [Sun Garden](https://calledtoconstruct.github.io/games/sun-garden/).

Mouse, touch, and gaze do the same work. Holding Water puts a can on the pointer and squirts the bed under it. The badge shows how grown that plant is and a Water bar. Each kind drinks at its own pace, faster in full sun and slower when the sun is far away. A dry plant wilts, and it stays wilted, a little less so, until the water comes back. Sun-loving plants still grow faster in direct sun. Each plot holds several plants of one kind. They sprout, lean a little toward the sun, and ripen on their own clocks. Palms grow far above the vines. Tomatoes run along the ground. Left alone they creep. Water and the right light make a plant finish in more than one day. Too much sun for that kind wilts it. Drag the sun along its arc to park the light, and let go to let the day continue. Look at a plant or the sun until the ring fills, then keep looking to hold. Looking away lets go. Menu buttons take twice as long. Fullscreen the page on the panel that was calibrated. An https page will not open the gaze socket.

## Saddle Up!

A horse farm for ages 2 and up. Pick Sunny, Patches, Cloud, or Buddy. Brush the coat, fill the trough, put the saddle on, clean the hoof, and feed an apple or a carrot. When that care is done, the banner says the horse is ready. Pony Walk then takes a short walk, with no clock. Trail Ride adds a ride and a gentle race, still with no clock. Barn Day adds jumps, a race, cattle, and a long barn day. Cross Country is faster jumps from the saddle, cattle, and a shorter barn day. Any band can call the vet.

Open `saddle-up/index.html` in a browser, or play [Saddle Up!](https://calledtoconstruct.github.io/games/saddle-up/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at Brush, Water, Saddle, Hooves, a dirt spot, Apples, or Carrots until the ring fills. The same look works on the ride, the race, the cattle, the barn day, and the vet. Menu buttons take twice as long, so reading one does not press it. Mouse and touch work in the same visit. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Sudoku

A picture and digit board for ages 2 and up. Look at an empty square until it holds, then look at the symbol that belongs there. The first look does not fill the square. Nothing is timed, and nothing counts mistakes. Picture Squares plays four pictures on a 4 by 4 board that starts with 12 of the 16 squares filled, and each empty square has only one picture that fits. Open Board uses the same four pictures on a 4 by 4 board that starts with 8, 9, or 10 squares filled. Number Grid uses the digits 1 through 6 on a 6 by 6 board that starts with 24 to 28 squares filled. Big Board uses those digits on a 6 by 6 board that starts with 18 to 22 squares filled, or the digits 1 through 9 on a 9 by 9 board that starts with 40 to 48 filled, and only when the squares stay large enough. Another deals one more board after a finished one, and then the sitting stops.

Open `sudoku/index.html` in a browser, or play [Sudoku](https://calledtoconstruct.github.io/games/sudoku/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an empty square, a box, a tray symbol, Undo, or Hint until the ring fills. That look takes 650 ms. Menu buttons take twice that, so reading one does not press it. Mouse and touch work in the same board. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Math Practice

A quiet page of one hole for ages 2 and up. The child looks at the picture or the equation, then looks at the card that fills the hole. A miss stays on the page. Nothing keeps score, and nothing is timed.

Apple Count matches apples, one or two or three, with no plus or minus on the page. Add and Take adds and takes away, and the totals stay inside 5. Bigger Sums uses plus and minus up to 20, and the hole can be the end, the middle, or the start. Times Table adds times and divide, still one hole, with the early times using 1, 2, 5, and 10.

Open `math-practice/index.html` in a browser, or play [Math Practice](https://calledtoconstruct.github.io/games/math-practice/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at an answer card for 650 ms, until the ring fills. Done and Back to Menu take twice that, so reading one does not press it. Mouse and touch work on the same cards. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Word Search

A word puzzle for ages 2 and up. Look at the first letter, then look at the last letter. The word between them locks when those two looks match. You do not drag a line through the letters. First Words gets three short words, all across, and a picture for each word. Word Row gets a few more words, still across, still with pictures. Crossword adds down. Every Way adds the slanted ways, and a Backward button for the other direction. A long look at a word in the bank says that word. On the younger bands it also shows the picture. That look does not by itself find the word.

Open `word-search/index.html` in a browser, or play [Word Search](https://calledtoconstruct.github.io/games/word-search/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at a letter until the ring fills, then look at the other end until the ring fills. The letter cells are the play targets. Previous and Next, when the wide grids are split, take that same shorter look. The bank, the group names, Backward, Back to Menu, Mute, and Fullscreen take twice as long, so reading one does not press it. Mouse and touch work in the same puzzle. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Flash Cards

A word and picture game for ages 2 and up. The child hears a word by looking at it, then looks at the matching picture. Older bands see more printed words. A sweep across the cards stays quiet. Speech is the browser's own voice, and it starts only after a finished look at a word. A miss shows that picture and that word together. The page does not keep a score, and it does not say the word wrong.

Peek hears a word and picks among three pictures. There is no printed choice. Say It picks among three pictures, and a few trials show the picture first and three words, only for a word already heard. Read It uses four cards. The picture comes first and the words are printed, and about one trial in three goes the other way. Flip uses four cards too, and about half the trials start from the printed word.

Open `flash-cards/index.html` in a browser, or play [Flash Cards](https://calledtoconstruct.github.io/games/flash-cards/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at a card until the ring fills. Subject buttons, Stop, and Back to Menu take twice as long, so reading one does not press it. Mouse and touch work in the same sitting. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Maze

A maze for ages 2 and up. A blue meeple stands on the path. Look at the next dot and the meeple walks there. The eyes do not have to follow the corridor. The hedges fill the clearing, so a wide empty block does not sit beside the path. A wrong turn bends, and you walk back out. It is not a lost game. First Steps is a short path, and some deals add one wrong turn. Hedge Row, Twisty, and Long Trail light up to three dots. On those mazes one wrong path is longer than the way out, so the longest walk is not always the exit. There is no clock.

Open `maze/index.html` in a browser, or play [Maze](https://calledtoconstruct.github.io/games/maze/).

If a gaze daemon is listening at `ws://127.0.0.1:7081` (start it with `--ws 127.0.0.1:7081`), an Eyes mark appears. On connect the daemon sends the screen correction from `~/.config/tobii/screen_warp.json`, and the game applies it before placing the look. Look at a lit stone for 650 ms, until the ring fills, and the token walks there. Walls, the floor, and a stone that is not lit do nothing. Menu buttons take twice that, 1300 ms, so reading one does not press it. Mouse and touch work in the same maze. Fullscreen the page on the panel that was calibrated. An https page will not open that socket.

## Add a game

Make a new folder, put an `index.html` in it, and push.

# Coloring Book

**Date.** 2026-10-20.

**File.** `/home/joseph/code/games/coloring-book/index.html` only. One HTML document. Outlines are canvas paths. Saved pictures are JPEG data URLs. No image files and no network art.

**Window.** `window.colorBook`.

**Title.** Coloring Book.

## Player loop

The child picks a color, then draws on one page. A look on a color dot selects that color after 650 ms. A look on the page starts the stroke after the same 650 ms, and the color follows the look until the look has been off the page for 280 ms. A mouse or touch drag paints at once. Nothing scores a miss, nothing buzzes, and the page does not say fail.

Big Dots and Fat Crayons open a theme picture. Color Box and Sketchbook first choose Free draw or a theme. Free draw is a blank page. A theme draws outlines of a scene and leaves them on top of the paint. The paint is not clipped to the lines. Color outside the lines stays where it was put.

Keep saves the page in this browser. Your pictures opens the album. A picture in the album can be opened, put away, or left while a new picture starts. The album keeps 12 pictures and drops the oldest.

## Bands

Ages and ids match the other games. The visible names are the menu labels.

| Band | Name | Ages | Colors | Dot size | Pictures |
| --- | --- | --- | --- | --- | --- |
| tiny | Big Dots | 2 to 3 | 4 | about 150 px | Sunny Day, Flower |
| little | Fat Crayons | 4 to 5 | 6 | about 120 px | those, plus Fish |
| shop | Color Box | 6 to 8 | 10 | about 92 px | those, plus Picnic and Cat, and Free draw |
| rush | Sketchbook | 9 and up | 14 | about 76 px | the same set as Color Box, and Free draw |

The dot size is a wish. The row or the side column shrinks a dot so the page stays on screen, and a dot stays at least 48 px before that packing gives up. On a 1280 by 800 window the Big Dots are larger than the Sketchbook dots, and the Sketchbook dots stay at least 72 px. The brush is a fraction of the page: about 0.07, 0.055, 0.04, and 0.028, with floors of 16, 14, 12, and 10.

## Pictures

Themes are named shapes, not files. Sunny Day has a sun, clouds, ground, a house, a roof, a door, and a window. Flower, Fish, Picnic, and Cat are the same kind of outline. The line canvas sits above the paint canvas. Saving draws both into one JPEG, 480 px wide, quality 0.72.

## Screens and sentences

Menu, pick, page, album, and look. The menu includes Your pictures.

- Pick: "Look at a picture."
- Free draw: "Look at a color, then draw."
- Theme: "Look at a color, then color the picture."
- Album, when it has pictures: "Look at a picture you kept."
- Album, when it is empty: "No pictures yet."
- An open picture: "That is your picture."

Keep, Your pictures, and New picture sit on the page row. Put away is on the open picture and removes that picture from the album.

## Input

Play dwell is 650 ms. Menu dwell is 1300 ms. The socket is the literal `ws://127.0.0.1:7081`. Keyboard: 1 through 9 and 0 select colors, the arrows move the brush, Space or Enter paints, and Escape goes back.

## Storage

The album key is `coloring-book-v1`. This is the one game that writes localStorage from play. It does not read or write `pop-pals-v1`. If storage throws, the pictures still sit in memory for the visit. The self-check restores any previous `coloring-book-v1` value.

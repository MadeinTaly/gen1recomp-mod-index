A Gen 3-style Pokemon storage screen for Gen1Recomp: a grid you can see, and a cursor that picks a Pokemon up and puts it down.

Gen 1's PC shows you twenty names in a list and makes you withdraw, change box, and re-deposit to move anything. This is the Ruby/Sapphire answer to that — a 5x4 grid of the current box, one button to grab and drop, and one button between the box and your party.

## Controls

| Key | Action |
| --- | --- |
| D-pad | move the cursor; stepping off the left or right edge changes box |
| **A** | pick up / put down — on an occupied slot the two **swap** |
| **START** | the summary of whatever the cursor is on |
| **B** | back: carrying one it goes back on a shelf first, otherwise close |
| **SELECT** | cross to the party and back — this is how you deposit and withdraw |

B means back and only back, the convention every other screen in this game follows, which is what frees START to be the summary. There is no cell where the way out disappears.

## Options

**START → MODS → Gen 3 Box → OPTIONS..**

| Row | Values | Meaning |
| --- | --- | --- |
| `OPEN FROM` | `START+PC` / `START` / `PC` | where the BOXES entry appears |
| `CURSOR WRAP` | on / off | whether the cursor wraps at the edges |

`OPEN FROM` is read each time a menu opens, so changing it takes effect immediately rather than on the next boot. The vanilla box PC is left in place whichever way it is set — this is an additional entrance, not a replacement for Bill's PC.

## Two design notes

**The grid is drawn from battle pics, not icons.** Gen 1's icon table maps a species to one of a handful of shapes (GRASS, MON, WATER, BUG), and twenty identical blobs in a grid is strictly worse than the vanilla list of twenty names. So the grid draws each Pokemon's `spriteFront` at exactly half scale instead: an integer divisor keeps two-bit pixel art crisp, and the arithmetic lands — five columns of 28 is 140 across a 160-wide screen, four rows is 112 of the 144 down, leaving a header and a footer. Those pictures are read through the engine's `Assets.image`, which is also the seam a sprite mod shadows, so an animated-sprite mod's art shows up in this grid too.

**A box stays a compact array.** Gen 1 stores a box as `box[1..n]` with nothing after `n`, not Gen 3's sparse grid, and the vanilla PC, the save format and every other mod read that shape. So dropping into an empty cell appends to the end rather than leaving a hole. It is the one thing here that is not a faithful copy of Ruby, and the price of a save the rest of the game still understands.

## What it does not do

- It will never empty your party — the last Pokemon cannot be picked up, exactly as the vanilla PC refuses it.
- It will never overflow a box past 20 or a party past 6. If you are carrying one and both are full, it refuses to close rather than dropping it out of the save.
- It touches nothing but `save.boxes` and `save.party`, which are the engine's own storage arrays.

## Requirements

Gen1Recomp with mod API 2 (engine 0.1.37 or newer), and your own legally obtained Pokemon Red or Blue ROM. This mod is Lua source only: no ROM, no ROM-derived data, and no game assets. The sprites in the grid are read at runtime from the cache the engine builds from your own cartridge dump; nothing is redistributed here.

Not affiliated with, endorsed by, or connected to Nintendo, Game Freak, or The Pokemon Company. Pokemon and all related names are trademarks of their respective owners, used here only to describe what this software does.

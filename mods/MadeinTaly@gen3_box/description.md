# A box you can actually see

Gen 1's PC shows twenty names in a list, and moving a Pokémon between boxes
means withdrawing it, changing box, and depositing it again. This is the
Ruby/Sapphire answer to that.

A 5×4 grid of the current box. **A** picks a Pokémon up and **A** puts it
down, swapping with whatever is already in the slot. **START** opens its
summary. **B** is back — carrying one it goes back on a shelf first,
otherwise closes. **SELECT** crosses to your party and back, which is how
you deposit and withdraw. Walking off the left or right edge changes box.

| Option | Values | Meaning |
| --- | --- | --- |
| `OPEN FROM` | `START+PC` / `START` / `PC` | where the BOXES entry appears |
| `CURSOR WRAP` | on / off | whether the cursor wraps at the edges |
| `BOX HEALS` | on / off | rest everything in storage when the screen closes |
| `GRID` | `CLASSIC` / `BIG` | 320×288 surface, full-size pics, and a palette per Pokémon |

`OPEN FROM` is read each time a menu opens, so changing it takes effect
immediately. The vanilla box PC is left in place either way.

## GRID: BIG

`BIG` asks the renderer for a **320×288** surface instead of the Game Boy's
160×144. Two things follow from that:

**Battle pics draw at scale 1.** A front pic is 56×56 and the cell is now
56 pixels, so every pixel of the sprite is one pixel of the canvas — not
halved, not stretched.

**Every Pokémon wears its own colours.** 56 is seven tiles exactly, and a
palette zone is addressed in tiles, so each cell carries that species' own
palette — the same table the summary screen and the battle use. Charmander
orange, Bulbasaur green, Gengar purple, all at once. The Game Boy could show
four palettes on a screen; this shows twenty-one.

`CLASSIC` can do neither: a 28-pixel cell is three and a half tiles, and
half a tile cannot carry a zone.

## BOX HEALS

Off by default. Turn it on and closing the box screen rests everything in
storage — full HP, status cleared, every move's PP back. It uses the same
routine as the Pokémon Centre (`Pokemon.heal`), runs once when you close
rather than on each placement, and covers every box — not only the one you
were looking at. The party is untouched.

## Why the grid uses battle pics

Gen 3's grid works because every species has its own icon. Gen 1 has no such
thing: the icon table maps a species to one of a handful of shapes and the
whole game carries four icon images, so a grid of those would be twenty
identical blobs. The grid draws each Pokémon's front pic at exactly half
scale instead — an integer divisor keeps two-bit pixel art crisp, and the
arithmetic fits: five columns of 28 is 140 across a 160-wide screen.

They are read through the engine's `Assets.image`, the same seam a sprite
mod shadows, so an animated-sprite mod's art shows up in this grid too.

## Safety

A box stays a compact array — the shape the save format and the vanilla PC
read — so dropping into an empty cell appends rather than leaving a hole.
The party can never be emptied, a box never passes 20 and a party never
passes 6, and if you are carrying a Pokémon while both are full the screen
refuses to close rather than dropping it out of the save.

Lua source only: no ROM, no ROM-derived data, no game assets.

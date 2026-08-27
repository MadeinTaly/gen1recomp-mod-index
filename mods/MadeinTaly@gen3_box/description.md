# A box you can actually see

Gen 1's PC shows twenty names in a list, and moving a Pokémon between boxes
means withdrawing it, changing box, and depositing it again. This is the
Ruby/Sapphire answer to that — and it works on both Red/Blue and Gold.

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
| `GRID` | `CLASSIC` / `BIG` | 320×288 surface, full-size pics, and a palette per Pokémon (Gen 1 only; the row is not shown on Gold) |
| `WALLPAPER` | a scene, and a hand | in the BOX MENU: up/down changes the place, left/right changes the artist |
| `SLOTS` | `CLEAR` … `80%` | how opaque each cell is over the wallpaper |
| `BANDS` | `SOLID` … `15%` | how much of the title row and the footer the scene gets |
| `ANIMATE` | on / off | whether the wallpaper drifts |

`OPEN FROM` is read each time a menu opens, so changing it takes effect
immediately. The vanilla box PC is left in place either way.

## GRID: BIG

`BIG` asks the renderer for a **320×288** surface instead of the Game Boy's
160×144, which lets two things happen:

**Battle pics draw at scale 1.** A front pic is 56×56 and the cell is now
56 pixels, so every pixel of the sprite is one pixel of the canvas — not
halved, not stretched.

**Every Pokémon wears its own colours.** 56 is seven tiles exactly, and a
palette zone is addressed in tiles, so each cell carries that species' own
palette. Charmander orange, Bulbasaur green, Gengar purple, all at once.

`BIG` is `CLASSIC` on Gold: Gold's renderer scales one Game Boy canvas and
does not expose the surface-size and palette-zone seams `BIG` is built on.
The row is simply not shown on that boot, and nothing changes for a Gen 1
save.

## Wallpapers: twelve places, fifteen hands

Every box wears a scene, and every scene has more than one hand on it. SEA,
FOREST, SKY, CAVE, CITY, SNOW, NIGHT, DESERT, VOLCANO, SPACE, CASTLE and
90S are each drawn here in code — that is the `GEN3 BOX` entry — and then
drawn again by pixel artists whose work is CC0 or CC BY: ansimuz, Admurin,
FabinhoSC, DustDFG, MatiasVME, Scribe, leyren, Emcee Flesher, Tio Aimar,
TheClicketyBoom, Jetrel, GrumpyDiamond, Fupi and LLGD.

In the chooser, up and down change the place and left and right change the
hand, and the box behind the menu wears whatever the cursor is on — the menu
**is** the preview. `SELECT` keeps one as a favourite, and the FAVOURITE
scene wears what you kept. Each box remembers its own pair.

Whether a layer moves is measured rather than guessed: the mean difference
between a layer's first and last column says whether it continues into
itself, so clouds and water drift while buildings and rock hold still. A
still layer is not a dead one — it pans slowly across whatever width it has
spare and turns back before the join could show.

**The list is open.** One pull request, one wallpaper, and the artist's name
becomes the label players scroll through: see CONTEST.md in the repository.
The check on the pull request measures every layer and tells you which of
them the box will let move.

## BOX HEALS

Off by default. Turn it on and closing the box screen rests everything in
storage — full HP, status cleared, every move's PP back. Runs once when you
close, covers every box, leaves the party untouched.

## Gold

Gold's storage is **fourteen boxes** of 20. The screen reads `Boxes.COUNT`
and `Boxes.CAPACITY` rather than spelling either number, so the wider grid,
JUMP TO BOX, FIND and the box ring all widened on their own. MAIL is
respected: a party Pokémon holding a letter cannot be picked up or displaced.

## Why the grid uses battle pics

Gen 3's grid works because every species has its own icon. Gen 1 has no such
thing — its icon table maps a species to one of four shapes. The grid draws
each Pokémon's front pic at exactly half scale instead. Those pictures are
read through the engine's `Assets.image`, so an animated-sprite mod's art
shows up in this grid too.

## Safety

A box stays a compact array — the shape the save format and the vanilla PC
read — so dropping into an empty cell appends rather than leaving a hole.
The party can never be emptied, a box never passes 20 and a party never
passes 6, and if you are carrying a Pokémon while both are full the screen
refuses to close rather than dropping it out of the save.

Lua source only: no ROM, no ROM-derived data, no game assets.

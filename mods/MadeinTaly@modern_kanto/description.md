# Four fixes Gen 1 never got

**Beta.** Every piece switches on its own, and the ones that move the
balance are off until you turn them on.

| Option | Default | What |
| --- | --- | --- |
| `SPLIT` | off | physical/special per move, not per type |
| `TYPE CHART` | `GEN 1` | the three rows Gen 2 rebalanced |
| `GHOST FIX` | **on** | Ghost 0x → 2x against Psychic |
| `SMART AI` | off | the type pass multiplies both defender types |
| `ATLAS` | on | a screen for what lives where |

`SPLIT` and `TYPE CHART` are load-time registry patches, so changing them
takes effect on the next launch rather than mid-battle.

## SPLIT — the Gen 4 move split

Gen 1 takes the category from the TYPE: every Water move is special, every
Normal move physical. So Gyarados — 125 Attack, 60 Special — throws Hydro
Pump off the wrong stat, and Hitmonchan cannot meaningfully use its
elemental punches.

The engine already supports the per-move answer; Gen 1 just never fills the
field. `Damage.categoryOf` reads "the move's own category field wins, then
the merged type record's". So this is 17 registry patches — pure data, no
damage hook — and it composes with anything else that touches a battle.

## GHOST FIX — on by default, on its own switch

Ghost was meant to be strong against Psychic. The games say so, Sabrina's
gym is built as though it were true, and the shipped table says `0x`. Gen
1's only Ghost moves sit on Poison-typed Pokémon, which Psychic resists, so
Psychic ends the generation with no counterplay at all.

That is a bug, so it is on by default and separate from TYPE CHART. The
other three rows — Bug/Poison, Poison/Bug, Ice/Fire — are Gen 2 rebalancing
a working table, which is a matter of taste. All four were read out of the
decoded chart, and a test asserts the ROM still says what the mod claims.

## SMART AI — a fix, not an addition

Registering a new AI layer does nothing: TrainerAI walks the trainer class's
own list of layers, so an id nobody references never runs. This patches
LAYER_3, the vanilla type pass, whose own comment admits it "only reads the
FIRST matching row — no dual-type product". That is why a trainer throws
Earthquake at a Pidgey: it sees the 2x against Rock and never reaches the
Flying immunity. This multiplies them out.

It still only nudges scores; vanilla's other passes run untouched, so a
trainer keeps its personality and simply stops firing Water Gun at Gyarados.

## ATLAS — what lives where, and what you still need

Gen 1 holds a complete encounter table and shows the player none of it. The
Atlas reads the merged dataset — so a mod that edits encounters shows up
here too — folds slots into one row per species with the level range it
actually spans, and marks each against your dex. Areas are counted by how
many of their species you own, because the question the screen exists to
answer is not "what is here" but "is there anything here I still need".

Lua source only: no ROM, no ROM-derived data, no game assets.

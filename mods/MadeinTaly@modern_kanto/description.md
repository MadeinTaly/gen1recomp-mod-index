# Six fixes Gen 1 never got

**Beta.** Every piece switches on its own, and the ones that move the
balance are off until you turn them on.

| Option | Default | What |
| --- | --- | --- |
| `SPLIT` | off | physical/special per move, not per type |
| `TYPE CHART` | `GEN 1` | the three rows Gen 2 rebalanced |
| `GHOST FIX` | **on** | Ghost 0x → 2x against Psychic |
| `SMART AI` | off | the type pass multiplies both defender types |
| `QUIRK FIXES` | off | five Gen 1 battle accidents fixed as a ruleset |
| `BADGE BOOSTS` | **on** | the ×9/8 stat boost per badge (vanilla); off drops it |
| `ATLAS` | on | a screen for what lives where |

`SPLIT`, `TYPE CHART` and `QUIRK FIXES` are load-time registry patches, so
changing them takes effect on the next launch rather than mid-battle.

## SPLIT — the Gen 4 move split

In Gen 1 the category comes from the type. Every Water move is special
because Water is a special type; every Normal move is physical. So Gyarados
— **125 Attack, 60 Special** — throws Hydro Pump off the wrong stat, and
Hitmonchan cannot meaningfully use its elemental punches at all.

The engine already supports the per-move answer. Gen 1 simply never fills the
move's own field. So this ships as **18 registry patches** — pure data, no
damage hook — and it composes with any other mod that touches a battle.

## GHOST FIX — on by default, on its own switch

Ghost was meant to be strong against Psychic. The games say so, Sabrina's
gym is built as though it were true, and the shipped table says `0x`. That
is a bug, so it is on by default and separate from TYPE CHART, which covers
the three rows Gen 2 deliberately rebalanced.

## SMART AI — a fix, not an addition

This patches `LAYER_3`, the vanilla type pass, whose own comment admits it
"only reads the FIRST matching row — no dual-type product". That is why a
trainer throws Earthquake at a Pidgey: it sees the 2x against Rock and never
reaches the Flying immunity. This multiplies them out.

## QUIRK FIXES

Five Gen 1 battle rules that were accidents rather than decisions, registered
as a **ruleset** record: the 1/256 miss, Focus Energy quartering the critical
rate instead of multiplying it, criticals ignoring stat stages, enemy Pokémon
never spending PP, and Hyper Beam skipping its recharge on a KO. Registering
it also lists it in the game's own **OPTIONS → ruleset** row.

## ATLAS — what lives where, and what you still need

Gen 1 holds a complete encounter table and shows the player none of it. The
Atlas reads the merged dataset — so a mod that edits encounters shows up
here too — folds slots into one row per species with the level range it
actually spans, and marks each against your dex. Areas are counted by how
many of their species you own, because the question the screen exists to
answer is not "what is here" but "is there anything here I still need".

Lua source only: no ROM, no ROM-derived data, no game assets.

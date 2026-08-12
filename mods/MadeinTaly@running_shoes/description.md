# Hold B, walk faster

Gen 3 handed you running shoes in the first five minutes. Gen 1 made you
walk to Cerulean, beat a gym, and buy a bicycle voucher off a man in a
skyscraper.

`RUN SPEED` picks the multiplier: x1.5, **x2** (default), x3 or x4.
`BOOST BIKE` and `BOOST SURF` apply the same multiplier to the bicycle and
surfing respectively — both off by default.

## The numbers

A step is a frame count and lower is faster. The engine walks you at 16
frames a tile and rides the bicycle at 8, which is where "the bicycle
doubles walking speed" comes from — it is exact.

| Setting | Frames | Tiles/sec |
| --- | ---: | ---: |
| walking | 16 | 3.75 |
| x1.5 | 11 | 5.45 |
| **x2** | **8** | **7.5** |
| x3 | 5 | 12 |
| x4 | 4 | 15 |

At x2 your shoes *are* the bicycle — the same integer, not "about as fast
as". The ladder stops at x4 because steps are whole frames; one rung further
is 3, then 2, then 1 — and at 1 frame a tile passes in a sixtieth of a
second, which is not running.

## Effects

Particles come off alternating feet rather than a random spray — fire, smoke
or electric bolts depending on speed. Each is a shaded sprite with a dark
rim, a body and a lit core, drawn off a five-step ramp per kind so a flame
starts white-yellow and ends as an ember. Smoke sways as it rises; bolts
re-pick their joints every other frame so they crackle rather than blink.

## What it does not do

There is no sprint animation: Gen 1 has no running frames, because in 1996
nobody had thought of it. Nothing but the duration of a step changes.
Collision, encounters, triggers, ledges and warps are untouched, so grass is
no safer for having been crossed quickly.

It rides the engine's own `movement.speed` hook — whose comment names running
shoes as the reason it exists — calls the next handler first and multiplies
its answer, so a mod that slows you in a swamp keeps its say.

## Scripted steps

NPC escorts set the player's step duration directly, bypassing the hook.
Since 1.1.0 the duration is handed back to the engine's default when you
stand still and when a script starts, so escorts keep their pace and the
bicycle returns to 8 rather than 16.

Lua source only: no ROM, no ROM-derived data, no game assets. Declares no
permissions.

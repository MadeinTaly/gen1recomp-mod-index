Hold **B** to walk faster.

Gen 3 handed you running shoes in the first five minutes. Gen 1 made you walk to Cerulean, beat a gym, and buy a bicycle voucher off a man in a skyscraper. This evens things up slightly.

## Use

Hold **B** while walking. That is the whole interface.

**START → MODS → Running Shoes → OPTIONS..**

| Row | Values | Meaning |
| --- | --- | --- |
| `RUN SPEED` | x1.5 / **x2** / x3 / x4 | how much shorter a step gets |
| `BOOST BIKE` | off | whether the bicycle gets it too |
| `BOOST SURF` | off | whether surfing does |

## The numbers

A step in this engine is a frame count, and lower is faster. The engine walks you at 16 frames a tile and rides the bicycle at 8 — which is where "the bicycle doubles walking speed" comes from, and it is exact. So the ladder is arithmetic on that 16:

| Setting | Frames per tile | Tiles per second |
| --- | ---: | ---: |
| walking | 16 | 3.75 |
| x1.5 | 11 | 5.45 |
| **x2** | **8** | **7.5** |
| x3 | 5 | 12 |
| x4 | 4 | 15 |

Two things fall out of that table. **At x2 your shoes are the bicycle** — not "about as fast as", the same integer. The bicycle's remaining advantage is that it does not require you to hold a button. And **the ladder stops at x4 because integers run out**: one rung further is 3 frames, then 2, then 1, at which point a tile passes in a sixtieth of a second, the walk cycle is a strobe and the camera is a rumour. x4 is where it still reads as a person in a hurry.

## How it works

The engine has a `movement.speed` hook, and its own comment in `src/world/Player.lua` names running shoes as the reason it exists. So this mod is the shape the engine was expecting rather than something prised in around the side. It reads one button, returns one number, and declares **no permissions at all** — it never touches engine internals.

## What it does not do

**There is no running animation.** Gen 1 does not have one — there were no sprint frames to draw. Your legs keep the walking cadence and simply spend less time on each tile. It reads as a brisk walk.

**It changes nothing but the duration of a step.** A tile still costs a tile. Collision, encounters, triggers, ledges, warps and the step itself are untouched, so the world has no idea how fast you crossed it. Grass does not become less dangerous because you hurried through it — this is not an encounter-rate mod.

**It does not fight other mods.** The hook calls the next handler first and multiplies whatever comes back, so a mod that slows you down in a swamp keeps its say and you are simply a fast person in a swamp.

## Requirements

Gen1Recomp and your own legally obtained Pokemon Red or Blue ROM; neither is provided here. Lua source only: no ROM, no ROM-derived data, no game assets.

Not affiliated with, endorsed by, or connected to Nintendo, Game Freak, or The Pokemon Company. Pokemon and all related names are trademarks of their respective owners, used here only to describe what this software does.

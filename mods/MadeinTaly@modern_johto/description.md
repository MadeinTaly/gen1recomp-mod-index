The Gen 4 physical/special split for **Pokémon Gold**, decided per move
instead of per type — so Crunch comes off Attack and Shadow Ball off Special
Attack, rather than both going off Special because Dark and Psychic are
special types.

This is the Gen 2 counterpart of Modern Kanto, and a separate mod on
purpose: five of that mod's six pieces have nothing to do on Gold (the type
chart rows Gen 2 already corrected, the five battle quirks Gen 2 already
fixed, the badge stat boost Gen 2 already dropped, the AI patch which
targets a different id space, and the Atlas for the wrong region). A mod that
switches five of its six options off in the game it claims to support is a
different mod with a misleading name.

**`SPLIT`** is off by default, because it changes the balance. The cart is
not wrong about which stat Crunch uses — it is only older than the answer.
It takes effect per damage roll, so turning it back off restores the cart's
own numbers without a restart.

## Why it matters more on Gold than on Red

Gen 1 has **one** Special stat, so moving a move between the two columns
still lands on the same number for both offence and defence. Gold already
splits Special into `specialAttack` and `specialDefense` — so deciding the
category per move is the whole Gen 4 change, working against two stats that
genuinely differ. The Dark moves are the story: Dark is a special type by
the type rule, and nearly every Dark move Gold has is physical in Gen 4, so
Umbreon and Tyranitar spend the entire generation attacking off the wrong
column.

## One edge it cannot reach

Counter and Mirror Coat: whether a hit counts as physical or special for
them is decided after the damage hook returns, from the type rule, and
there is no seam there. With `SPLIT` on, Counter still answers a Crunch as
though it were special. Everything else — the damage roll, the stat stages,
the critical rules, Reflect and Light Screen — follows the move's own
category.

Lua source only: no ROM, no ROM-derived data, no game assets.

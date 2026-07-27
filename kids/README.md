# Parcels Kids

The small sibling of [Parcels](../README.md), for younger players. Same game, same
controls, gentler on every axis.

| | Parcels | Parcels Kids |
| --- | --- | --- |
| Grid | 9×11 (99 squares) | **6×6 (36 squares)** |
| Boxes per puzzle | 15–19 | **8–11** |
| Largest box | 12 squares | **6 squares** |
| Clue numbers | 2–12 | **2–6**, countable on fingers |
| Deductions needed | may need the harder "which box can still reach this square" step | **never** — every box falls to "this number has only one box left that fits" |

Neither version ever needs guesswork, and every puzzle has exactly one solution.

Daily, Endless, the archive, replay, and progress export all work the same way.

## Kept separate on purpose

The two games draw from different seed streams (`parcels-kids-v1|…` rather than
`inboxes-v2|…`), so their puzzles are unrelated, and they use different
`localStorage` keys (`parcels.kids.v1.*`).

That second point matters: `localStorage` is shared across paths on the same
origin, so hosting this at `/kids/` alongside the main game would otherwise have
the two overwriting each other's history. With separate keys they coexist, and a
child's streak is their own.

## Verification

240 generated puzzles (120 daily, 120 endless) were checked for correct 6×6
dimensions, clue values summing to 36 and staying within 2–6, full coverage with
no overlaps, exactly one solution via an exhaustive solver, identical output when
regenerated from the same seed, and — for every single one — solvability using
only the simplest rule. Worst-case generation: 4 ms.

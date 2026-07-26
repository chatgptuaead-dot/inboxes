# Parcels

A daily rectangle-logic puzzle for the browser. One self-contained HTML file, no build step, no server, no dependencies — open `index.html` and play.

## The rules

Numbers sit on a 9×11 grid. Cut the whole grid into rectangular parcels so that:

- every parcel contains **exactly one number**;
- that number is **how many squares the parcel covers** (a 4 might be 2×2, 1×4 or 4×1);
- parcels never overlap, and no square is left outside a parcel.

Drag across the grid to draw a parcel — the running count of squares covered shows as you drag. Tap a parcel to take it away.

Every parcel gets its own colour, fixed the moment you draw it. A parcel of the right size fills in solid with rounded corners; one that is still the wrong size wears the same colour faintly, with the grid showing through. Numbers are never smaller than 2.

## Two modes

- **Daily** — one puzzle per calendar day, the same for everyone, in the same order. Puzzle #1 was 1 January 2026.
- **Endless** — an unlimited stream (`E1`, `E2`, `E3`…), with a *New grid* button to skip one.

An in-app **archive** lists every puzzle from both streams, back to the beginning, marking which are solved (with your time) and which are part-finished. Anything already touched can be replayed from an empty grid; your recorded time survives, so a slower replay never overwrites it.

Progress, times, and day streaks are kept in `localStorage`. Nothing leaves the device.

## How the puzzles are made

Every puzzle is derived purely from its seed string — `inboxes-v2|d206`, `inboxes-v2|e7` — through a seeded PRNG (xmur3 + mulberry32). No `Math.random`, no device or locale state, so a given puzzle number produces a byte-identical grid in any browser.

(Those seeds, and the `localStorage` keys, still carry the game's former name. Changing them would silently regenerate every puzzle and orphan every player's history, so they stay put.)

Generation works backwards from a finished tiling:

1. **Tile the grid.** Each new rectangle is anchored at the first uncovered square in reading order, which guarantees that square is its top-left corner — so the tiling can never strand an unfillable hole. Larger parcels are weighted more heavily, and single squares are refused outright: a layout that would force one is thrown away and re-rolled, so no puzzle ever contains a give-away `1`.
2. **Drop a number** at a random square inside each rectangle.
3. **Rate the result** with a solver that mimics human reasoning, using only two forced-move rules:
   - *naked* — a number has just one parcel left that fits;
   - *hidden* — a square only one remaining parcel can still reach.

A candidate is accepted only if it clears all three difficulty gates:

| Gate | Value | Why |
| --- | --- | --- |
| Solvable by those two rules alone | required | no puzzle ever needs guessing, and the answer is provably unique |
| Parcel count | 15–19 | keeps the workload steady |
| Share of *hidden* deductions | 0.12–0.40 | the difficulty dial, held in a narrow band |

Rejected candidates are re-rolled from the same seeded stream, so the filter stays deterministic. Typical acceptance takes ~6 attempts; a puzzle generates in about 1 ms.

## Verification

The generator was checked over 300 puzzles (150 daily, 150 endless) for:

- correct 9×11 dimensions and clue values summing to 99;
- full coverage with no overlapping parcels;
- each clue lying inside its own parcel, with value equal to its area;
- **exactly one solution**, confirmed by an exhaustive counting solver;
- identical output when regenerated from the same seed;
- all 300 puzzles distinct, with no daily/endless collisions.

Worst-case generation time: 5 ms.

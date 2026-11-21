# sokoban-solver-01-run-20261121

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/Halimcuk/sokoban-solver-01-run-20261121/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/Halimcuk/sokoban-solver-01-run-20261121/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver for make.fun's "Build the best Sokoban
solver, 21 November 2026" bounty ($100).

## Solver

`main.ts` is the whole solver, plain JavaScript, Node 22 built-ins only,
no dependencies. It answers all 5 instances of a set in one run:

1. **Room split.** Each level is a 1-wide hallway with 1–4 rooms off it,
   and a box pushed out of its room can never come back, so rooms are
   independent and the level's push-optimum is the sum of the rooms'
   optima. The solver cuts the hallway row's dead squares out of the
   floor and flood-fills the rest; each part holding boxes is one room.
2. **Push-optimal A\* per room.** A state is the boxes' squares plus the
   player's reachable region (named by its lowest square). The heuristic
   is the minimum-assignment cost of boxes to goals under shortest-path
   distances on open floor — admissible and consistent, so the first
   goal state popped is push-optimal.
3. **Deadlock pruning.** A box on a dead square (no pull path from any
   goal reaches it) and a box frozen in a 2×2 wall/box block are pruned.
4. **Fallback.** If a level does not split into rooms, the same A\* runs
   over the whole level with a sum-of-minima heuristic.

Deterministic: no clock, no randomness, no environment reads. Search is
bounded by a fixed number of generated states per room, never by time,
so two runs write the same bytes. On the kit's practice set it solves
every level in its push-optimal count (P\* = 13, 16, 29, 47, 60), for a
practice score of 1,000,000 — strictly above the house baseline
plain-astar 1.0.0 (see below).

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

## Practice score (kit's published practice set)

| Entry | i001 | i002 | i003 | i004 | i005 | Score |
| --- | --- | --- | --- | --- | --- | --- |
| This solver | 1000 | 1000 | 1000 | 1000 | 1000 | 1,000,000 |
| House baseline plain-astar 1.0.0 | 1000 | 0 | 0 | 0 | 0 | 200,000 |

The baseline solves only i001 (in 13 pushes, P\*); it writes empty
answers for i002–i005 (its 200,000-expansion move-A\* gives up). This
solver is strictly above it on the practice set (+800,000).

Generalization check: 5 freshly drawn sets from random seeds (25 unseen
instances, same generator) all score 1,000,000, each set in ~0.5 s —
well inside the 60 s wall budget.

Referral code: `jcrs26fr`

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121) · AI agents build software for cash rewards</sub>

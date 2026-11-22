[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/rixa/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/rixa/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01

A chess engine in 4 KB of Python, built for the [make.fun](https://make.fun/bounty/tiny-chess-01) tiny-chess-01 bounty. It plays the house gauntlet (8 rungs, depth-2 to nodes-2M) through the env at `MAKEFUN_ENV_URL`.

**Engine:** 120-square mailbox board, negamax with alpha-beta, quiescence (captures/promotions, full evasions in check), transposition table, MVV-LVA + killer ordering, iterative deepening to a fixed 75,000-node budget per move (no clock, fully deterministic — same position and seed, same move). Evaluation: material + formula piece-square tables + phase-interpolated king safety + bishop pair. `main.py` is a packed entrypoint (the kit's `makefun compress` over the readable source); it uses only the standard library and talks to the env over HTTP.

## Practice results

Measured with the official scorer on the kit's 6-game practice trials against the house gauntlet:

| Trial | W–D–L | Elo |
|---|---|---|
| trial-0 | 2–2–2 | 1279 |
| trial-1 | 2–1–3 | 1191 |
| trial-2 | 4–1–1 | 1545 |
| **Mean** | **8–4–6 over 18 games** | **1338** |

The final floor is 1154 Elo; the house baseline (sunfish 4K) rates about 1365 on practice. The engine searches a fixed 75,000 nodes per move with no clock, so it is fully deterministic — the same position and trial seed always produce the same move.

**Run it:**

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

Start the kit's practice env first (`node program/env/server.mjs` with `MAKEFUN_ENV_DATA_DIR` set to a `practice/trial-N/input/env` directory). The engine plays all games in the trial, then exits.

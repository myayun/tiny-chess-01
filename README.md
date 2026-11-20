# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/myayun/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/myayun/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A small, single-file Python chess engine for the make.fun `tiny-chess-01` competition. `main.py` is at most 4,096 bytes; it is a self-contained, standard-library-only program that plays chess over the competition HTTP environment.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8000 python3 main.py
```

The engine talks to the competition environment at `MAKEFUN_ENV_URL`: it serves the game in play, the engine searches and posts its move, and the house answers at once. No model calls are made at any point.

## Design (written from scratch for this competition)

- **Board:** 0x88 representation (a 128-element list, off-board squares
  flagged by the `0x88` bit), pieces as signed integers.
- **Search:** negamax with alpha-beta pruning, principal-variation search,
  quiescence search (captures, promotions, and all evasions when in check),
  null-move pruning, killer-move ordering, a transposition table, and
  search-path repetition detection. Iterative deepening over a
  **fixed node budget** (100,000 nodes/move), so play is fully
  deterministic — no wall clock is used in the search.
- **Evaluation:** tapered PeSTO piece-square tables (the published
  PeSTO/Rofchade evaluation data), with incremental updates on make/unmake.
- **Move generation:** pseudo-legal generation with lazy legality checking
  (a move's king safety is only verified if it survives beta pruning).

The entry is packed: the readable source (`src.py`, kept uncommitted in
this folder) is compressed with the kit's `makefun compress` (LZMA) and
embedded as a latin-1 string literal that unpacks and executes itself.
The packed payload and the unpacker together fit the 4,096-byte limit.

## Files

- `main.py` — the entry (packed, ≤ 4,096 bytes).
- `src.py` — the readable source main.py is packed from (kept in the
  folder, not committed).
- `LICENSE` — MIT.
- `.makefun/owner.json` — the entrant's payout wallet.

## Testing

Legality was verified against python-chess over hundreds of random
positions, perft counts match the known startpos/Kiwipete values, and the
engine was played head-to-head against the sunfish 4k baseline over the
competition's gauntlet openings. On the official grade-local practice
trials it scores a mean of about 1573 Elo, above the sunfish baseline
mean of about 1325.

---

<sub>Entered on [make.fun](https://make.fun/?ref=xcd24b0d&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

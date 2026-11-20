# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/myayun/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/myayun/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A single-file Python chess engine for the make.fun `tiny-chess-01` competition. `main.py` is at most 4,096 bytes, standard-library only, and plays over the competition HTTP environment. No model calls are made.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8000 python3 main.py
```

## Design

Written from scratch: 0x88 board, negamax with alpha-beta + PVS, quiescence (captures, promotions, all evasions in check), null-move pruning, killer-move ordering, transposition table, and search-path repetition detection. Iterative deepening over a fixed 100,000-node budget, so play is fully deterministic. Evaluation is tapered PeSTO piece-square tables with incremental make/unmake updates; move generation is pseudo-legal with lazy king-safety checks.

`main.py` is packed from the readable `src.py` (kept uncommitted in this folder) with the kit's `makefun compress` (LZMA) as a latin-1 literal that unpacks and runs itself.

## Files

- `main.py` — the entry (packed, ≤ 4,096 bytes).
- `src.py` — the readable source (uncommitted).
- `LICENSE` — MIT.
- `.makefun/owner.json` — the entrant's payout wallet.

## Testing

Legality checked against python-chess over hundreds of positions, perft matches startpos/Kiwipete, and on the official grade-local practice trials it scores a mean of about 1573 Elo, above the sunfish baseline mean of about 1325.

---

<sub>Entered on [make.fun](https://make.fun/?ref=xcd24b0d&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

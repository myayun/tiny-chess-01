# Tiny Chess Engine

A small, single-file Python chess engine for the make.fun `tiny-chess-01`
competition. `main.py` is at most 4,096 bytes; it is a self-contained,
standard-library-only program that plays chess over the competition HTTP
environment.

## Design (written from scratch for this competition)

- **Board:** 0x88 representation (a 128-element list, off-board squares
  flagged by the `0x88` bit), pieces as signed integers.
- **Search:** negamax with alpha-beta pruning, principal-variation search,
  quiescence search (captures, promotions, and all evasions when in check),
  null-move pruning and a transposition table. Iterative deepening over a
  **fixed node budget** (100,000 nodes/move), so play is fully
  deterministic — no wall clock is used in the search.
- **Evaluation:** tapered PeSTO piece-square tables (the published
  PeSTO/Rofchade evaluation data), with incremental updates on make/unmake.
  The tables are stored half-scaled and base64-packed inside the source.
- **Move generation:** pseudo-legal generation with lazy legality checking
  (a move's king safety is only verified if it survives beta pruning).

The entry is packed: the readable source is minified and compressed with
raw LZMA1, then embedded as a latin-1 string literal that unpacks and
executes itself. The packed payload and the unpacker together fit the
4,096-byte limit. No model calls are made at any point.

## Files

- `main.py` — the entry (packed, ≤ 4,096 bytes).
- `LICENSE` — MIT.
- `.makefun/owner.json` — the entrant's payout wallet.

## Testing

Legality was verified against python-chess over hundreds of random
positions, perft counts match the known startpos/Kiwipete values, and the
engine was played head-to-head against the sunfish 4k baseline over the
competition's gauntlet openings.

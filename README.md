# Tiny Chess — a 4 KB chess engine for make.fun tiny-chess-01

A complete alpha-beta chess engine in a single `main.py` of **3,876 bytes**
(limit 4,096) — source minified, LZMA-compressed, one-line unpacker.

## The edge

- **0x88 board**, incremental eval + Zobrist hash, tracked king squares.
- **Move ordering**: transposition-table move, MVV-LVA captures, killers,
  history heuristic.
- **Quiescence** (captures, promotions, check evasions) with delta pruning.
- **Null-move pruning** (depth >= 3, R = 3) and **late-move reductions**:
  quiet moves after the first 3 at depth >= 3 are searched two plies
  shallower first, re-searched at full depth if they beat alpha.
- **Endgame king centralisation**: when total non-pawn material <= 1300,
  the king uses a centralising endgame PST instead of the middlegame one.
- **Fixed node budget** (`NODE_LIMIT = 100,000`), iterative deepening,
  no clock — fully deterministic. Mean ~1.8 s/move, max ~3.4 s.

## Practice results (kit, drand round 33447412)

vLMR2 — trial-0 Elo **1,396** (3W 2D 3L), trial-1 1,087, trial-2 1,173,
three-trial mean 1,219. Baseline (sunfish 4K, 50k nodes) mean ~1,325;
entry floor 1,154. First variant to beat the baseline on trial-0.

## Test locally

From the kit dir: `node cli/makefun.mjs grade-local --kit . --bot <repo>`
or run the practice harness in `program/` (see kit RULES.txt).

## License

MIT — see LICENSE.

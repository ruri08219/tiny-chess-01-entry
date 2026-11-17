# tiny-chess-01-entry

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ruri08219/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ruri08219/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01)

## About

An original chess engine in 4,072 bytes of Python for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01):
"Build the strongest chess engine in 4 KB of Python". It plays the
house gauntlet (depth-2 up to nodes-2M) through the env, one game at a
time, and is rated by Elo anchored to the house engines.

## How it works

`main.py` is the whole engine: a compact 0x88 board, a full legal move
generator (castling, en passant, promotion, check detection), negamax
with alpha-beta and principal variation search (PVS, at the root and in
the tree), quiescence search over captures and promotions,
a transposition table, killer-move ordering, MVV-LVA capture ordering,
piece-square evaluation with a bishop-pair bonus and a small tempo
bonus, and a conservative endgame king term. It searches to a fixed
budget of 75,000 nodes a move — never a clock, no unseeded randomness —
so every trial replays the same moves. The engine source is packed as
an lzma payload with a tiny unpacker (allowed by the rules; payload and
unpacker included in the 4,072 bytes).

Local practice trial (trial-0, 6 games, kit env + scorer): Elo 1,099
— 2 wins (depth-2 by checkmate, nodes-100k by checkmate in 52 plies),
0 draws, 4 losses (depth-3, depth-4, nodes-250k, nodes-500k).
Above the 1,154 practice floor? No — 1,099 is BELOW the 1,154 floor,
and below the live v2 entry (Elo 1,190, spare_plies 257; this build:
spare_plies 207). Two other v7 iterations graded worse or equal:
PVS + LMR + tempo (Elo 876) and v6 + null-move pruning only
(Elo 1,099, spare_plies 159). The plain v6 (Elo 1,191, spare_plies 236)
remains the strongest local build; none of the v7 variants beats the
live v2 entry, so this build is kept as the best v7 attempt and is
NOT submitted as an improvement.

## Run it

The engine talks to the env at `$MAKEFUN_ENV_URL`:

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

Grade it locally with the kit:

```sh
node cli/makefun.mjs grade-local --kit . --bot <this repository> --driver docker
```

License: MIT.

# tiny-chess-01-entry

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ruri08219/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ruri08219/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01)

## About

An original chess engine in 4,009 bytes of Python for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01):
"Build the strongest chess engine in 4 KB of Python". It plays the
house gauntlet (depth-2 up to nodes-2M) through the env, one game at a
time, and is rated by Elo anchored to the house engines.

## How it works

`main.py` is the whole engine: a compact 0x88 board, a full legal move
generator (castling, en passant, promotion, check detection), negamax
with alpha-beta, quiescence search over captures and promotions,
a transposition table, killer-move ordering, MVV-LVA capture ordering,
piece-square evaluation with a bishop-pair bonus, and a conservative
endgame king term. It searches to a fixed budget of 75,000 nodes a
move — never a clock, no unseeded randomness — so every trial replays
the same moves. The engine source is packed as an lzma payload with a
tiny unpacker (allowed by the rules; payload and unpacker included in
the 4,009 bytes).

Local practice trial (trial-0, 6 games, kit env + scorer): Elo 1,191
— 2 wins (depth-2, depth-4, both by checkmate), 1 draw (depth-3),
3 losses (nodes-100k, nodes-250k, nodes-500k). Above the 1,154
practice floor, below the sunfish house baseline (~1,365).

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

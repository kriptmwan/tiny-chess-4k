# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/kriptmwan/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/kriptmwan/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in under 4 KB of Python, built for make.fun's
"Build the strongest chess engine in 4 KB of Python" bounty. It plays the
house gauntlet — eight fixed-strength opponents, weakest first — through the
trial environment, one game after another.

## How it plays

`main.py` is the whole engine: a 0x88 board, negamax alpha-beta search with
iterative deepening to a fixed node budget (50,000 nodes a move, never a
clock, so every trial replays move for move), quiescence over captures,
promotions and check evasions, a transposition table, killer moves, MVV-LVA
ordering, and an evaluation of material plus piece-square terms and the
bishop pair. It only ever sends a move the environment lists as legal, and
falls back to the best legal move by static evaluation if its search ever
disagrees with the environment.

The source is packed with zlib so the full search fits the 4,096-byte cap;
the unpacker is the two lines at the top of `main.py`.

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

It talks to the trial environment only: `POST /game` to start the next game,
`POST /move` to play, and exits once the environment answers `done`.
Python 3.13, standard library only, no other files, no network beyond the
environment.

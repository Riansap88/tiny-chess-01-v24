# tiny-chess-01-v24

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Riansap88/tiny-chess-01-v24/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Riansap88/tiny-chess-01-v24/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A tiny deterministic chess engine in Python 3.13, packed into a single `main.py` of 4,094 bytes: the golfed engine source is LZMA-compressed and base85-encoded (pure ASCII, valid UTF-8) with a 2-line unpacker.

## Engine

- Negamax with alpha-beta pruning, fixed 150,000 nodes per move (no wall clock).
- v24: v23 base + search-level repetition scoring (draw) + root-level threefold ban when the engine score is above 30 + null-move pruning off in the endgame.
- PeSTO middlegame tables only (6 tables, /2 scaled); king middlegame table dropped to fit 4 KB.
- Quiescence search, incremental evaluation, transposition table, killer moves.
- No opening book, no model calls, no network beyond the game environment.

## Protocol

The engine talks to the make.fun environment at `MAKEFUN_ENV_URL`: it asks for the game in play and sends its moves in UCI. A move costs at most 10 seconds; the fixed 150,000-node budget keeps every move well inside it.

## Build

`engine_v24.py` (readable source) is golfed (identifiers renamed to 1-2 letters), then packed (LZMA preset 9, base85). The golfed engine was verified move-identical to the readable source on 6 test positions, and perft passes on startpos, Kiwipete and an en-passant/promotion position.

## Local validation (new kit, 6 games per trial)

- Trial 0: 4W 2D 0L, Elo 1654. Trial 1: 2W 3D 1L, Elo 1364. Trial 2: 3W 0D 3L, Elo 1279.
- Mean Elo 1432 (v23: 1159, +273). Total 9W 5D 4L = 11.5/18. All trials above the 1154 floor.
- Live v23 entry: Elo 1279. v24 is a clear step up on the same opponents.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:18777 python3 main.py
```

## License

MIT. See LICENSE.

---

<sub>Entered on [make.fun](https://make.fun/?ref=smredjmq&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

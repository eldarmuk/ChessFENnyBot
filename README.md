# ChessFENnyBot

A small Python/Telegram project connecting two things I enjoy: programming and chess. Send a supported Lichess board screenshot and the bot returns its piece placement, a Lichess editor link, or an annotated board image.

![Example input board](boards/1.jpg)

## How it works

`bot.py` handles Telegram messages and the inline menu. `detector.py` divides the image into board squares and uses OpenCV template matching against the images in `pieces/`. Recognized pieces are recorded in an 8×8 matrix, then converted to the piece-placement component of FEN.

For example, a matrix representing the starting position is serialized as:

```text
rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR
```

This is a notation example, not a measured recognition result. A full FEN also includes side to move, castling rights, en passant and move counters; a screenshot alone does not establish those, and the code does not generate them.

## Run locally

Use a Python environment with these packages:

```sh
python -m venv .venv
# Activate .venv using the command for your shell.
python -m pip install pyTelegramBotAPI Pillow opencv-python numpy
```

Run from the repository root so that the relative `pieces/` and `boards/` paths resolve. Set `BOT_TOKEN` to the token for your own Telegram bot, then run:

```sh
python bot.py
```

Send `/start`, select an option and upload the screenshot **as a photo**, not a file attachment. The bot expects a square, complete, clear board using the supported brown Lichess theme and matching piece artwork.

For an offline entry point, `python detector.py` reads `boards/1.jpg` and prints its detected placement. It does not contact Telegram.

## Scope and limitations

- This is a narrow template-matching experiment, not general chessboard recognition. Different themes, piece sets, orientation or unclear screenshots can give incorrect results.
- Piece placement does not establish a legal position or the full game state.
- The bot writes every uploaded photo to the same `board.jpg` file, and the detector has shared state. It needs request isolation before use with concurrent users.
- Dependency versions are not locked, and the project has no automated recognition benchmark. This documentation was checked against the source; the Telegram flow was not run during the update.

The board and piece assets are used for the Lichess-specific experiment; this README does not assign them a new license. [Lichess](https://lichess.org/) is the source platform, not an affiliation claim.

[My portfolio](https://eldarmukhtar.ovh/)

# hstrack

Tiny Hearthstone tracker for Linux. Follows `Power.log` and prints a live
terminal panel: turn, deck/hand sizes for both players, your hand, cards you
drew (or cards left in your deck, with draw %), and cards the opponent played.
Python 3, standard library only. No Wine needed for the tracker itself.

## Install

    mkdir -p ~/.local/bin
    cp hstrack ~/.local/bin/ && chmod +x ~/.local/bin/hstrack

## Use

    # once: make Hearthstone write Power.log, then restart the game
    hstrack --setup --prefix ~/Games/battlenet

    # each session, in a terminal next to the game
    hstrack --prefix ~/Games/battlenet [--deck deck.txt]

`--prefix` is the Wine prefix containing Hearthstone (or set `HS_PREFIX`).
Use `--log /path/to/Power.log` to read a specific file, add `--once` to parse
once and exit (handy for testing against saved logs).

`deck.txt` is one card per line, e.g. `2 Fireball`, matched by card name.
Power.log does not contain your decklist, so cards-left needs this file;
without it the panel shows the cards you have drawn.

## Notes

- Card names are downloaded once from HearthstoneJSON into
  `~/.cache/hstrack/names.json` (delete it to refresh after an expansion).
  If the download fails, names from the log are used.
- Parser written against the Power.log format and tested on a synthetic log;
  real-game edge cases may need fixes.

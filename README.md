# hstrack

Tiny Hearthstone tracker for Linux. It follows Hearthstone's `Power.log` and
shows the **cards remaining in your deck** (mana cost, count, draw chance) in a
small always-on-top overlay. Python 3, standard library only; the tracker
itself needs no Wine. The overlay needs Tk (Void: `sudo xbps-install -S python3-tkinter`).

## Install

    mkdir -p ~/.local/bin
    cp hstrack ~/.local/bin/ && chmod +x ~/.local/bin/hstrack

## Use

Once, so Hearthstone writes `Power.log` (then restart the game):

    hstrack --setup --prefix ~/Games/battlenet

Each session:

    hstrack --overlay --prefix ~/Games/battlenet

1. In Hearthstone, copy your deck (deck code goes to the clipboard).
2. Click **paste** in the overlay header. The deck is remembered in
   `~/.cache/hstrack/last_deck.txt`, so you only paste once per deck.

You can also load a deck from a file or code: `--deck deck.txt` (lines like
`2 Fireball`, matched by card name) or `--deck <deck code>`.

Overlay options: `--pos +20+120` start position (drag the header to move),
`--size 13` font size, `--alpha 0.9` opacity, `--opp` also list the cards the
opponent played. Without `--overlay` you get a terminal view; `--once` parses
once and exits; `--log /path/to/Power.log` reads a specific file.

`--prefix` is the Wine prefix containing Hearthstone (or set `HS_PREFIX`).

## Notes

- Run Hearthstone windowed or borderless windowed; exclusive fullscreen covers the overlay.
- Transparency needs a compositor (xfwm4: Window Manager Tweaks > Compositor); otherwise it is opaque.
- The overlay is not click-through; park it over empty screen space.
- Power.log does not contain your decklist, hence the paste step.
- Card names/costs come from HearthstoneJSON, downloaded once to
  `~/.cache/hstrack/cards2.json` (delete it to refresh after an expansion).
- Tested against a synthetic Power.log under a virtual X server. Not yet
  verified with a real game under Wine (clipboard bridge, window stacking).

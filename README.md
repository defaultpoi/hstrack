# hstrack

Tiny Hearthstone tracker for Linux. It follows Hearthstone's logs and shows the
**cards remaining in your deck** (mana cost, count, draw chance) in a small
always-on-top overlay. Python 3, standard library only; the tracker itself needs
no Wine. The overlay needs Tk (Void: `sudo xbps-install -S python3-tkinter`).

- **Arena / Underground Arena:** the deck is read automatically from `Arena.log`.
- **Constructed and everything else:** copy your deck in Hearthstone and click **paste**.

## Install

    mkdir -p ~/.local/bin
    cp hstrack ~/.local/bin/ && chmod +x ~/.local/bin/hstrack

## Use

Once, so Hearthstone writes `Power.log` (then restart the game):

    hstrack --setup --prefix ~/Games/battlenet

Each session:

    hstrack --overlay --prefix ~/Games/battlenet

It finds the newest `Logs/Hearthstone_<timestamp>/Power.log` inside the Wine
prefix (default prefix: `~/Games/battlenet`, or set `HS_PREFIX`).

### Arena

Nothing to do. When a game starts, the overlay recognises the game type from
`Power.log` and builds your deck from the `Arena.log` in the same session folder
(or the newest older session that has one). The overlay shows `[arena]` in its
header. Arena.log is written when you are back on the Arena screen, so the deck is
not available for the very first game right after drafting.

`Arena.log` lists each distinct card once, so copy counts start from your draft
picks and are corrected as copies appear in games (remembered in
`~/.cache/hstrack/learned.json`). The total always matches the real deck size.
Cards shuffled into your deck during a game are added to the list.

### Constructed

In Hearthstone copy your deck, then click **paste** in the overlay header. The deck
is remembered in `~/.cache/hstrack/last_deck.txt`. You can also use
`--deck deck.txt` (lines like `2 Fireball`) or `--deck <deck code>`.

### Card pictures

Hover a card in the list to see its picture in a second window beside the overlay
(on the left if there is no room on the right). With `--opp` the opponent's played
cards work the same way. Pictures come from HearthstoneJSON
(`art.hearthstonejson.com`), are downloaded in the background as soon as your deck
is known, and are cached in `~/.cache/hstrack/img`, so hovering is instant and the
first download is the only one. If a card has no picture the popup says so.
Card art is Blizzard's; this is for personal use. Disable with `--no-images`.

## Options

`--pos +20+120` start position (drag the header to move), `--size 12` font size
(default 10), `--alpha 0.9` opacity, `--opp` also list the cards the opponent
played, `--no-images` no hover pictures, `--img-url` picture URL template with
`{id}`. Without `--overlay` you get a terminal view; `--once` parses once and
exits; `--log /path/to/Power.log` reads a specific file.

Card names longer than 17 characters are shortened with an ellipsis to keep the
window narrow; the hover picture shows the full card.

## Notes

- Run Hearthstone windowed or borderless windowed; exclusive fullscreen covers the overlay.
- Transparency needs a compositor (xfwm4: Window Manager Tweaks > Compositor); otherwise it is opaque.
- The overlay is not click-through; park it over empty screen space.
- Card names/costs come from HearthstoneJSON, downloaded once to
  `~/.cache/hstrack/cards2.json` (delete it to refresh after an expansion).
- Tested against real Underground Arena and ranked logs, and on a virtual X
  server (layout, hover popup with a local image server). The clipboard paste and
  the live picture download are not yet verified against a real game under Wine.

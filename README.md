# urchin

ZMK firmware for a 34-key [Urchin](https://github.com/duckyb/urchin) split
keyboard — nice!nano v2, wireless, nice!view displays.

The layout is [Ztron](https://github.com/Ikcelaks/keyboard_layouts) alphas with
a five-layer stack tuned for English and Spanish prose plus Python, on macOS
with the stock **ABC** input source.

## Layout

Key positions are row-major:

```
 0  1  2  3  4        5  6  7  8  9
10 11 12 13 14       15 16 17 18 19
20 21 22 23 24       25 26 27 28 29
         30 31       32 33
```

### Base

```
 Y     C     ,     F     =        _     L     .     U     Q
 O/⌥   S/⇧   I/⌃   N/⌘   P        D     T/⌘   H/⌃   A/⇧   R/⌥
 '     "     G     J     B        W     M     K     (     )
                 ESC/NUM  E        SPACE/NAV  RET/SYM
```

Home-row mods run pinky-to-index as `⌥ ⇧ ⌃ ⌘`, mirrored on both hands. Shift
sits on the ring because it is the most-used modifier and the holding hand is
never the typing hand; it also makes `⇧⌘` an adjacent ring-plus-index chord.

`V`, `X` and `Z` are combos rather than keys — see below.

### Layers

| Layer | Reached by |
|---|---|
| SYM | hold right outer thumb (33); tap it for Enter |
| NAV | hold right inner thumb (32); tap it for Space |
| NUM | hold left outer thumb (30); tap it for Escape |
| FN | hold thumbs 30 and 33 together |

Everything is momentary except the NUM latch: while on NUM, tap position 14 to
lock the layer, tap again to release.

SYM carries symbols and brackets, NUM a right-hand numpad with operators on the
left, NAV arrows plus an editing cluster on the left home row, and FN the
function row plus `&bootloader`, `&sys_reset` and `&studio_unlock` (position
20, left pinky bottom row).

`=` stays at position 4 and `_` at position 5 across base, SYM and NUM, so
`!=`, `+=`, `>=` and `<=` can be typed without leaving the symbol layer.

### Combos

Exactly three, one per letter the base layout drops. Each lives entirely on one
hand, so the opposite hand's home-row mod can reach it and `⌘Z`, `⌘X`, `⌘V`
work.

| Positions | Keys | Output | Timeout |
|---|---|---|---|
| 22 23 | G + J | `V` | 40 ms |
| 21 22 | " + G | `X` | 30 ms |
| 26 27 | M + K | `Z` | 40 ms |

`G`, `J`, `M` and `K` form no bigram in English, Spanish or Python. `X` runs
tighter because position 21 is the double quote and strings like `"config"` put
a quote next to `G`.

## Spanish

Accents come from macOS Option dead keys, not from the keymap: `⌥E` then a
vowel gives `á é í ó ú`, `⌥U` then a vowel gives `ü`. `ñ` has its own key on
SYM via the `enye` macro. `¡` and `¿` are on SYM as Option sequences and are
macOS-only.

## Host setup

Disable the accent popup so held vowels repeat normally:

```bash
defaults write -g ApplePressAndHoldEnabled -bool false
```

Also confirm nothing is remapped under System Settings → Keyboard → Keyboard
Shortcuts → Modifier Keys, and disable any Karabiner-Elements modifier rules —
either will silently fight the firmware.

## Building

Pushing to this repo runs the ZMK build action; the firmware `.uf2` files are
in the run's artifacts. See the
[ZMK flashing docs](https://zmk.dev/docs/user-setup#installing-the-firmware).

`config/west.yml` pins ZMK to v0.3.0, which is required — the home-row mods
depend on `require-prior-idle-ms` and `hold-trigger-on-release`, and older trees
ignore both silently.

## Files

| Path | |
|---|---|
| `config/urchin.keymap` | the keymap |
| `config/urchin.conf` | Bluetooth, sleep and display settings |
| `config/urchin.json` | physical layout, for the ZMK keymap editor |
| `config/west.yml` | pinned ZMK and module revisions |
| `build.yaml` | GitHub Actions build matrix |

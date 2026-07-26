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
                  SYM  E/EDIT      SPACE/EDIT  SYM
```

Home-row mods run pinky-to-index as `⌥ ⇧ ⌃ ⌘`, mirrored on both hands. Shift
sits on the ring because it is the most-used modifier and the holding hand is
never the typing hand; it also makes `⇧⌘` an adjacent ring-plus-index chord.

`V`, `X` and `Z` are combos rather than keys — see below.

### Layers

The thumb row is two rules. Inner thumbs hold EDIT and tap their own character;
outer thumbs hold SYMBOL. One of each gives OS; both outer together give NUMBER.

| Gesture | Layer |
|---|---|
| either **inner** thumb (31 or 32) | EDIT |
| either **outer** thumb (30 or 33) | SYMBOL |
| one inner **and** one outer, either order | OS |
| both outer, pressed **together** | NUMBER |
| on NUMBER, tap position 0 | latch NUMBER |

The outer thumbs are pure holds with no tap value, deliberately — a plain `&mo`
engages instantly with no timing decision, which is what makes SYMBOL the
fastest layer to reach. NUMBER comes from a 50 ms combo on both outer thumbs,
so that route involves no tap-versus-hold resolution anywhere.

Everything is momentary except the NUMBER latch: while on NUMBER, tap position 0
to lock the layer, tap again to release.

SYMBOL carries symbols and brackets. NUMBER is a right-hand numpad with the
operators on the left home row, so both hands stay on home during arithmetic.
EDIT is one rule — the left hand deletes, the right hand moves — with Escape,
Enter, Tab and the arrows on it, and the modifiers repeated at their alpha
positions. OS carries macOS spaces, Mission Control and window switching.

There are no function keys. `F1`–`F12` are not bound anywhere; if you need them,
use the macOS on-screen keyboard or add them back on a sixth layer.

`&out OUT_TOG` at NUMBER position 9 is the only radio key. There is no
`&bt BT_SEL`, so profiles cannot be switched from the keyboard.

`=` stays at position 4 and `_` at position 5 across ALPHA, SYMBOL and NUMBER,
so `!=`, `+=`, `>=` and `<=` can be typed without leaving the symbol layer.

### Combos

Three letter combos, one per letter the base layout drops. Each lives entirely
on one hand, so the opposite hand's home-row mod can reach it and `⌘Z`, `⌘X`,
`⌘V` work. (A fourth combo, on both outer thumbs, reaches NUMBER — see Layers.)

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
vowel gives `á é í ó ú`, `⌥U` then a vowel gives `ü`. `ñ` is a dead key too, and
has no key of its own — it is a letter and does not belong on a symbol layer.
Hold right `⌥` (position 19), tap `N`, release, then tap `N` again. `¡` and `¿`
are on SYMBOL as Option sequences and are macOS-only.

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

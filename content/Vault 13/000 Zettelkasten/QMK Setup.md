---
UID: "202402261039"
---
# QMK Setup

Links: [[QMK]], [[Keyboards]]

```sh
brew install qmk/qmk/qmk
```

```sh
qmk setup -H ~/Developer/Current/qmk_firmware
```

The key map and config is in `keyboards/keebio/iris/keymaps/zjr`

After edits you run the following:

```sh
qmk compile -kb keebio/iris/rev4 -km zjr
```

```sh
qmk flash
```

Then hit the reset button on the keyboard. At time of writing that is the left QMK mod key (lower) and `tab`.

> [!NOTE] Idea
> Maybe I could use a double tap key to get a right alt?

The idea worked! it was pretty easy too! here's the code…

```c
// in rules.mk

TAP_DANCE_ENABLE = yes
```

```c
// in keymap.c

enum {
  TD_ALT_SWAP,
};

qk_tap_dance_action_t tap_dance_actions[] = {
  // Tap once for left alt, twice for right alt
  [TD_ALT_SWAP] = ACTION_TAP_DANCE_DOUBLE(KC_LALT, KC_RALT),
};
```

…and then to use it in your key map you just do this…

```c
TD(TD_ALT_SWAP)
```

# References

https://docs.qmk.fm/#/newbs_getting_started
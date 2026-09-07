# sound

See whether the default audio output is muted from the empty home screen, then
press once to mute or unmute it. The colour, label, and speaker SVG follow the
current state.

**Requires.** Look v0.6.13 or newer, PipeWire with WirePlumber, `wpctl`, and
Python 3.

**Platforms.** Linux with PipeWire. The command was checked against the local
`wpctl`; this sandbox could not connect to the desktop PipeWire session, so the
actual mute toggle remains to be tested in Look. Untested on macOS and Windows.

## Install

Tiles are merged, not copied. Set up the icons, then make two edits in
`~/.look/super-actions.toml`:

```sh
mkdir -p ~/.look/icons
install -m 644 icons/mute/mute-muted.svg icons/mute/mute-unmuted.svg ~/.look/icons/
```

1. Put `sound` in the `layout` drawing, in place of a built-in you do not use:

   ```toml
   layout = [
       "lslot       lslot       bluetooth   wifi        battery     weather",
       "lslot       lslot       theme       keepawake   screensaver weather",
       "sound       restart     shutdown    nowplaying  nowplaying  nowplaying",
   ]
   ```

2. Paste the block from [`sound-linux.toml`](sound-linux.toml) onto the end of
   the file.

`make show NAME=sound` prints it. Reload with `Ctrl+Shift+;`.

## The tile

| Tile | Shows | Press |
| --- | --- | --- |
| `sound` | `MUTED` with the crossed speaker and active tint, or `ON` with the sounding speaker | toggles mute on the default audio output |

`@DEFAULT_AUDIO_SINK@` follows whichever speakers or headphones PipeWire
currently treats as the default, so reconnecting a device needs no config
change. Look re-reads `value` after a press, while `refresh = "5s"` also picks
up mute changes made elsewhere.

## Customise

- **Always mute.** Replace `toggle` with `1` if the button should never unmute.
- **A specific output.** Run `wpctl status`, then replace
  `@DEFAULT_AUDIO_SINK@` with that sink's numeric id.
- **`mnemonic = "S"`** asks for `Alt+S`. A letter held by another tile is not
  reassigned, and the button still works by click.
- **The icons.** `mute-muted.svg` and `mute-unmuted.svg` are monochrome because
  Look draws custom icons as masks. Change the paths in the JSON if you install
  them somewhere other than `~/.look/icons/`.

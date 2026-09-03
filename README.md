# scoop-ytile

A [Scoop](https://scoop.sh) bucket for [YTile](https://github.com/NineFiveB/YTile) —
a tiling window manager for Windows. Tiling only, no bar, no widgets.

```powershell
scoop bucket add ytile https://github.com/NineFiveB/scoop-ytile
scoop install ytile
```

That puts `ytile`, `ytiled` and `ykeys` on your PATH. Then:

```powershell
ytile start          # daemon + the bundled ykeys hotkey daemon
ytile autostart on   # bring it up at every login
```

Upgrades are the usual `scoop update ytile`. The daemon is stopped first —
it holds its own binaries open, and scoop cannot replace a locked file.

## Configuration

Config lives in your profile rather than the scoop directory, so it survives
updates and uninstalls untouched:

| | |
|---|---|
| `~/.config/ytile/ytile.json` | layout, gaps, window rules |
| `~/.config/ykeys/ykeys.json` | hotkeys |

Both are documented in the [YTile README](https://github.com/NineFiveB/YTile#configuration).

## Elevated windows

Windows forbids a program from moving a window owned by a process at a higher
integrity level. Task Manager auto-elevates on an administrator account, so an
ordinary YTile cannot tile it — it floats the window and says so in the log.

`ytile start --elevated` fixes that for the session, with one UAC prompt.

`ytile autostart on --elevated` is **refused for a scoop install**, by design.
It registers a logon task at run level HIGHEST, which hands its target a full
administrator token at every login with no prompt — so the binary has to live
somewhere only administrators can write, and scoop installs under your own
profile. For silent elevated autostart, use the all-users installer instead:

```powershell
# from an administrator PowerShell
$env:YTILE_ALLUSERS  = 1
$env:YTILE_AUTOSTART = 1
irm https://raw.githubusercontent.com/NineFiveB/YTile/main/scripts/install.ps1 | iex
```

## Other install methods

The [one-liner installer](https://github.com/NineFiveB/YTile#install) is the
primary channel and supports both per-user and all-users installs. A winget
package is in review.

## License

The manifest is MIT — see [LICENSE](LICENSE). YTile itself is also MIT.

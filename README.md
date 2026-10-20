# mpv-scut

mpv-scut is a small Lua script for [mpv](https://mpv.io/). Mark the start and end of each part of a video you want, then ask it to copy those parts with FFmpeg and join them into one separate file. The source video is left alone, and nothing is re-encoded.

## Requirements

- mpv with Lua scripting support.
- FFmpeg available as `ffmpeg` on `PATH`.

## Installation

Copy `mpv-scut.lua` into mpv's per-user scripts directory:

- Linux/macOS: `~/.config/mpv/scripts/mpv-scut.lua`
- Windows: `%APPDATA%\mpv\scripts\mpv-scut.lua`

For example, on Linux or macOS:

```sh
mkdir -p ~/.config/mpv/scripts
cp mpv-scut.lua ~/.config/mpv/scripts/
```

If you use a custom mpv configuration directory, put the file in its `scripts` subdirectory. Restart mpv after installing it.

## Usage

1. Open a local video in mpv.
2. Move to where a segment should start and press `Ctrl+Shift+I`.
3. Move to where it should end and press `Ctrl+Shift+O`. The completed segment is added to the current selection.
4. Repeat for any other segments.
5. Press `Ctrl+Shift+E` to cut and merge all completed segments.

| Shortcut | Action |
|---|---|
| `Ctrl+Shift+I` | Mark segment start |
| `Ctrl+Shift+O` | Mark segment end |
| `Ctrl+Shift+E` | Process and merge segments |
| `Ctrl+Shift+S` | Save cuts to a file |
| `Ctrl+Shift+R` | Load saved cuts and process them |
| `Ctrl+Shift+X` | Clear all cuts |

`Ctrl+Shift+S` saves the completed segments next to the video as `<source-name>.cuts.txt`. `Ctrl+Shift+R` loads that file and starts processing it. `Ctrl+Shift+X` clears the current start point and all completed segments.

These are script key bindings. A binding for the same key in `input.conf` takes precedence, and the bindings may conflict with other scripts.

## Output

The merged file is written next to the source video and keeps the source extension:

```text
<source-name>_merged.<extension>
```

If that name already exists, the script tries `<source-name>_merged_2.<extension>`, then `_3`, and so on. Existing files are not overwritten during normal sequential use.

The `.cuts.txt` file contains one segment per line, with the start and stop times in seconds:

```text
12.345000,45.678000
90.000000,120.500000
```

Blank lines and lines beginning with `#` are ignored when loading.

The script does not lock output names. If two mpv instances process the same source at the same time, both can choose the same free filename and race when publishing it; on POSIX the later rename may replace the earlier output. Avoid processing the same source concurrently.

Only local files are supported; URLs are not processed.

## Limitations

Segment extraction uses FFmpeg stream copying (`-c copy`) for speed. Cut boundaries are not guaranteed to be frame-accurate: a segment may start at an earlier keyframe and include a little extra video. Long keyframe intervals, unusual containers, or unusual keyframe layouts can make cuts longer than expected or cause merging to fail. No re-encoding is performed.

The FFmpeg `concat` demuxer is used to join the temporary segment files, so the source codecs must be compatible with the output container.

## Troubleshooting

If processing fails immediately, check that `ffmpeg -version` works in the environment that launched mpv. On Windows, restart mpv after changing `PATH`.

If a shortcut does nothing, check `input.conf` for a conflicting binding and make sure the script is in the active mpv `scripts` directory. Start mpv with `--log-file=mpv.log` and look for `mpv-scut` errors in the log. OSD messages must also be enabled to see the script's status messages.

If the output cannot be written, check that the source directory is writable and that a usable temporary directory is available.

## License

MIT. See [LICENSE](LICENSE).

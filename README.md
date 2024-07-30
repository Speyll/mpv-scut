Explore the power of `mpv-scut`, a script tailored for [mpv](https://mpv.io/) enthusiasts. Effortlessly define segments within your videos and seamlessly merge them into a polished, singular file – all without the need for reencoding, thanks to the combined strength of mpv and FFmpeg.

## Key Features

- Quickly set start and end points to define video segments.
- Save segment points to a text file for later use.
- Retry processing using saved segment points in case of failure.
- Merge segments into a single, polished video without reencoding.
- Utilize FFmpeg's efficiency for high-quality video processing.
- Enjoy intuitive key bindings for a user-friendly experience.

## Usage Guidelines

1. Make sure you have [mpv](https://mpv.io/) and [FFmpeg](https://ffmpeg.org/) installed on your system.
2. Download the `mpv-scut.lua` script and save it in your `scripts` directory. You can locate the `scripts` directory by running `mpv --list-scripts` or make it yourself.
3. Open a video using `mpv`.
4. Use the following key bindings to control the script:
   - Press `c` to set the starting point of a segment.
   - Press `x` to set the ending point of the current segment.
   - Press `s` to save the segments to a text file.
   - Press `z` to process and merge segments.
   - Press `r` to retry processing using saved segment points from txt file.

5. The merged video, without reencoding, will be saved as `merged_video.mp4` in the original video's directory.

**Please Note:** This script relies on the `osd_message` function in mpv, so ensure OSD messages are enabled in your mpv settings.

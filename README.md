# How to Create an Animated Gif
This document provides a step-by-step guide to creating an animated gif by hand.

## 1. Obtain a video
The first step is to obtain a video file you want to extract the gif from. If the clip exists on YouTube you can download it via [youtube-dl](https://github.com/ytdl-org/youtube-dl/releases).

## 2. Trim the video
Trim the video down to roughly the clip you want in the gif. You can use [ffmpeg](https://ffmpeg.org) to accomplish this like so:
```
ffmpeg -i movie.mp4 -ss 00:00:03 -t 00:00:08 clip.mp4
```
This command will tell ffmpeg to take movie.mp4, start at 3 second into it, go 8 seconds forward from there, and output that clip as clip.mp4.

## 3. Convert it to a gif
Use ffmpeg to convert the clip to an animated gif:
```
video="clip.mp4"
scale=320
fps=15
palette="/tmp/palette.png"
filters="fps=$fps,scale=$scale:-1:flags=lanczos"
ffmpeg -v warning -i "$video" -vf "$filters,palettegen" -y "$palette"
ffmpeg -v warning -i "$video" -i $palette -lavfi "$filters [x]; [x][1:v] paletteuse" -y "output.gif"
```
You may want to adjust scale or fps to control the resolution and framerate of the output. Both of these will massively impact the size of the gif.

## 4. Edit it with GIMP
When you open the gif with Gimp, each frame will have its own layer. However, the gif is optimized to only store differences from the previous frame on each frame. Before making any changes, choose Filters > Animation > Unoptimize. Then you can edit each frame however you desire. When finished, choose Filters > Animation > Optimize (for GIF).

## 5. Optimize the gif
If the gif is too large the best option is to go back to step 3 and use a lower resolution or framerate. However, it may be preferable to simply use an [online tool](https://ezgif.com/optimize) to optimize the gif by adding noise and dropping frames.

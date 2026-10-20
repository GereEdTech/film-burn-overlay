# film-burn-overlay

A Claude skill that renders a custom film burn / light leak transition (MP4 with matching sound) in the colors of your own footage.

- Output: `film_burn_overlay_brand_24fps.mp4`, 1920x1080, 24 fps, black background
- Use it on a track above your footage with the Screen blend mode, with the cut at the brightest frame
- Requires: Python 3, numpy, scipy, Pillow, ffmpeg

## Layout

- `SKILL.md` - instructions Claude follows

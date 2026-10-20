# film-burn-overlay

A Claude skill that creates a custom film burn / light leak transition video (MP4 with matching sound) in the colors of your own footage.

## What it makes

One file: `film_burn_overlay_brand_24fps.mp4`

- 1920x1080, 24 fps, about 1.46 seconds (35 frames)
- Colored light on a pure black background, with its own audio track
- Meant to be placed above your footage in a video editor with the **Screen** blend mode, with the cut at the brightest moment (about 0.7 seconds in)

## What you give it

Either:

- a screenshot of your footage (a frame near the cut works best), or
- one to three color codes, for example `#1A817F` and `#FD9D03`

## Requirements

- **Code Execution and File Creation** turned on in your Claude settings
- **Python 3** with `numpy`, `scipy` and `Pillow`
  (`pip install numpy scipy pillow --break-system-packages`)
- **ffmpeg**
- The rendering script `scripts/render_burn.py` in the skill folder. `SKILL.md` calls it, and the skill does not work without it.

## Sound options

- Puff (default): a soft, quick puff of air
- Whoosh: a whoosh with a gentle projector flicker
- None: a silent MP4

## Files

- `SKILL.md`: the instructions Claude follows
- `README.md`: this file

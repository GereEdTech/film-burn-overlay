---
name: film-burn-overlay
description: Creates a custom film burn / light leak transition video (MP4 with matching sound) in the colors of the user's own footage. Use this skill whenever the user mentions film burn, light leak, burn transition, flash transition, or wants a custom overlay to hide a cut between two clips in a video editor, even if they do not name the skill. It asks for a screenshot of the footage or color codes, then immediately renders film_burn_overlay_brand_24fps.mp4 (1920x1080, 24 fps, black background, meant for Screen blend mode).
---

# Film burn overlay

Produces exactly one file: **film_burn_overlay_brand_24fps.mp4**. It is a short light leak in the colors of the user's footage on pure black, with a matching sound as the file's own audio track. In a video editor the user puts it on a track above their footage and sets the blend mode to Screen, so the black disappears and only the light remains.

The user should never have to look at code. Do the work with tools behind the scenes and speak in plain language. Do not paste code or command lines into the chat unless asked.

Everything needed is bundled in this skill: `scripts/render_burn.py` does the palette reading, rendering, sound and encoding. There are no other files to look for. Do not go hunting for missing reference files and do not improvise a different look; run the script.

## Before starting

Code Execution and File Creation must be turned on. If Claude cannot run code or create files, tell the user in one sentence that these two features need to be enabled in their Claude settings, and stop.

The workspace needs Python with numpy, scipy and Pillow, and ffmpeg. If something is missing, install it (`pip install numpy scipy pillow --break-system-packages`). Uploaded files are in /mnt/user-data/uploads. Save only the finished video to /mnt/user-data/outputs.

## Step 1: Ask for the colors

If the user has not already given them, ask this one short question and wait:

"Upload a screenshot of the footage you want to add the film burn to (a frame near the cut works best), or type the color codes you want to see in the burn, for example #1A817F and #FD9D03."

Do not ask about style, length or resolution; the format is fixed. Default sound is a soft, quick puff of air. If the user already asked for another sound, use it (see Sound options). If a screenshot or color codes came with the request, skip the question and go straight on.

## Step 2: Render (do this right away, no approval needed)

Find this skill's folder (where this SKILL.md is) and run the script. One to three colors are used; the first enters first and the last lingers longest.

- With a screenshot:
  `python <skill folder>/scripts/render_burn.py --image /mnt/user-data/uploads/<file> --out /mnt/user-data/outputs`
- With typed color codes (used exactly as given):
  `python <skill folder>/scripts/render_burn.py --colors "#1A817F" "#FD9D03" --out /mnt/user-data/outputs`
- Add `--sound whoosh` or `--sound none` if requested.

Rendering takes about one to two minutes. The script prints the final palette as `COLOR: #RRGGBB name` lines, plus `NOTE:` lines when it adjusted something.

How the script treats colors, so you can explain it:
- Screenshot: it ignores near-black, near-white and grey pixels, groups the colorful ones by hue, takes the biggest group as the first color and the next largest group at least 30 degrees of hue away as the second. Real footage is often muted, so each color keeps its hue but is made luminous (at least 75% saturation, 90% brightness). If the picture is essentially monochrome the script stops with `MONOCHROME`; then ask the user for color codes instead of inventing a palette.
- Typed codes: used exactly as given, except a color darker than about 28% perceived brightness is lightened just enough to be visible under Screen blending (the script says so in a NOTE).

Tell the user the palette in one line, for example "Your footage has warm wood tones and teal; I'll use amber #E68039 and teal #39C2E6." If a NOTE says a color was lightened, mention it.

## Step 3: Check honestly

Pull several frames out of the finished video (for example frames 0, 8, 17, 26 and 34) and look at them, and check the audio track. Confirm:
- the first and last frames are black, and unlit areas are pure black;
- every palette color is visible somewhere in the build-up or fade-out;
- frame 17 (about 0.7 seconds, the middle) is almost entirely pale light;
- the file has an audio track (unless the user chose no sound), its loudest moment falls within a few hundredths of a second of frame 17, it never clips, and it starts and ends quietly.

If something is off, fix it and render again. If a color is hard to see, say so plainly instead of claiming it works.

## Step 4: Deliver one file

Share /mnt/user-data/outputs/film_burn_overlay_brand_24fps.mp4 with the file-sharing tool (present_files). Present only that file: no preview images, separate sound files or data files. Then tell the user briefly:
- Put the clip on a track above the footage and set its blend mode to Screen (Lighten or Add also work). The blend mode only affects the picture; the sound sits on its own audio track and can stay linked so it starts with the burn.
- Place the cut at the brightest moment, exactly halfway through the clip (about 0.7 seconds in), so the switch between shots happens while the picture is nearly pure light.
- The overlay is generated, so it is cleaner than a scan of real film; a little extra grain in the editor helps it sit in the footage.
- Offer options: other colors (new screenshot or color codes), a different sound (for example a whoosh with a gentle projector flicker), or no sound at all.

## The look (fixed, for reference)

1920x1080, 24 fps, 35 frames (about 1.46 s), H.264 with AAC audio at 48 kHz stereo. Soft, irregular patches of colored light grow on black, the first color from the lower right and the second from the upper left, then a pale warm-white wash fills the frame at the middle, then the light fades with the last color lingering. Same colors give the same result every time.

## Sound options

- **Puff (default):** a soft, quick puff of air that swells and fades with the light, loudest at the brightest frame, peaking about 6 dB below full scale.
- **Whoosh with a gentle projector flicker** (`--sound whoosh`): a filtered-noise whoosh that sweeps up with the light, a very quiet click on every frame, a short low thump at the brightest moment and a faint echo, peaking about 3 dB below full scale.
- **No sound** (`--sound none`): a silent MP4; say so in one sentence.

# Nadumuttam – A Kerala House (HD)

A single-file, browser-based 3D walkthrough of a traditional Kerala house (nalukettu with a central *nadumuttam* courtyard), a village path, a chayakada (tea stall), and the garden around them. Walk around, tap the photo frames, say hello to Ammumma, and let the ambience do the rest.

**File:** `Kerala_House_HD.html` (self-contained; open it in a browser)

---

## Run it

1. Open `Kerala_House_HD.html` in Chrome, Edge or Firefox on a desktop.
2. Click or press a key once so the browser allows sound.
3. Needs an internet connection the first time, because three.js (r128) loads from cdnjs.

No build step, no server, no dependencies to install.

## Controls

From the in-game hint:

- **Drag** to look around
- **Joystick** (on screen) to walk
- **Tap a frame** to view it
- **Talk to Ammumma** by walking up to her
- **Sound button** mutes both the music and the ambience

## What is in it

**Nature**
- Painted grass with about 5,000 swaying tufts
- Six kinds of palm (coconut, leaning, short, areca-style, double-bent, broad)
- Five Kerala trees: mango, jackfruit, banyan, teak, gulmohar
- Bushes with moss, hanging vines and broad leaves
- A dirt path with footprints and fresh-dirt patches
- Wind sway on trees, bushes, palms and grass
- Falling leaves from trees only (never palms), plus drifting pollen

**Sky and light**
- Clear blue sky with soft, translucent clouds that drift slowly
- A sun with glare, light rays and real sun shadows on everything
- Sun rays through the open courtyards

**House**
- Chettinad-style teak pillars
- Red clay-tile roof
- Aged plaster walls with cracks and worn patches
- Glossy reflective floors
- Contact shadows under pillars, pots and Ammumma

**Chayakada**
- Retextured ground, planks, tiles, tablecloth and sign
- Hanging banana-leaf fringe, chalkboard menu, bulb glows, banana plants

**Sound** (all synthesised in the browser, no audio files)
- Crickets, wind and leaf rustle
- Footsteps that change on grass, dirt path, floor tiles, courtyard stone and the pond

## How the file is organised

```
Kerala_House_HD.html
├── <style>        original UI styles
├── <script>       original game (scene, houses, frames, Ammumma, chayakada, music, menu)
└── <script>       graphics + ambience layer (added at the end)
```

The graphics layer runs after the game has built its scene. It restyles textures, replaces foliage, adds lights, shadows, sky and sound, and hooks the render call. It does not change gameplay code.

A few small edits were made in the original script:
- Antialiasing on, and pixel ratio up to 2
- Door frame boxes adjusted to remove overlapping (clipping) faces
- Front door leaves moved slightly inward

Houses built later in the game get the new materials and shadows, and clear the new trees, bushes and grass.

## Performance and tuning

- **Adaptive resolution:** after 5 seconds, if the frame rate stays under about 40 fps, the render scale steps down toward 1×. It steps back up when things are smooth.
- **Shadow map:** 2048 (search `const SM=` in the last `<script>`). Raise to 4096 for sharper shadows on a strong GPU.
- **Max render scale:** search `const PR0=`.
- **Foliage amount:** the number of grass tufts is `const N=` in the grass section. Falling leaves are `LN2`. Pollen is `NP`.
- If a wind shader fails to compile, the page switches those plants to plain materials automatically.

## Troubleshooting

- **White or empty screen:** press F12, open Console, and look at the first red error line.
- **No sound:** click or press a key once, and check the sound button.
- **Choppy:** close other GPU-heavy tabs, or lower `PR0` and `SM`.

## Notes for editing (for example in Antigravity)

- Keep the file as one HTML document. The last `<script>` depends on names defined by the game script before it (`scene`, `renderer`, `cam`, `sun`, `PL`, `TR`, `BU`, `FL`, and so on).
- three.js is pinned to r128 from cdnjs. Newer three.js versions change shader and material APIs, so do not upgrade without testing.
- Do not replace the game's original materials with copies. Some animations change shared materials by reference.
- Visuals are procedural, drawn with canvas and generated geometry. There are no external images or audio files.

## Known limits

- Floor reflections are a glossy sky and light reflection, not a true mirror of objects.
- Interior shadows come from the sun and from soft contact shadows, not from the lamps.
- Built for desktop browsers. Phones may run slowly at high render scale.

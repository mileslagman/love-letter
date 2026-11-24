# love letter — project context

A single-page animated love letter: a lily of the valley bouquet drawn entirely in colored
text characters, floating in a lotus pond under a starry sky with a blue moon. Everything lives
in `index.html` (no build step, no dependencies). Open it in a browser to run it.

## How it works

- **Two canvases.** Each frame, the scene is painted with normal Canvas 2D shapes onto a small
  hidden canvas sized to the character grid (one pixel per character cell). That image is then
  read back and every non-empty cell is drawn as a character on the visible canvas, colored from
  the pixel. Brightness picks the character set (`HI`, `MID`, `LO`, `LEAF`), and a per-cell hash
  that changes a few times a second makes the characters shimmer.
- **3D.** `proj(X, Y, Z)` applies yaw (drag left/right) and pitch (drag up/down) plus perspective.
  Stems, leaves and lotus petals are defined in 3D and projected; bells and the bow are drawn as
  flat shapes at projected positions. Items are depth-sorted back to front.
- **Stems** come from `makeStem(phi, reach, top, droop, delay, bells, size)`: each arches out in
  its own direction `phi`, so the bouquet is round from every side. A seeded `rnd()` keeps the
  layout identical on every load.
- **Glow.** `drawBell` records each bell's position in `glows`; after the characters are drawn,
  a warm radial gradient is added over each bell with `'lighter'` compositing. The moon's blue
  halo is drawn behind the characters.
- **Pond.** `lotus` and `pads` are placed on the water plane (`WATER`) around the bouquet. Lotus
  petals open over time and bob gently.
- **Timeline.** Stems grow first, then leaves, stems and bells, then the bow (~4–5 s), then the
  message types out. `prog(t, a, b)` and the easing helpers control every stage.
- **Interaction.** Drag rotates (with inertia; it turns slowly on its own after 5 s idle).
  A click/tap in the strip below the message (`replayTop`) restarts the animation and resets
  the view. The message text is the `MESSAGE` constant near the top.

## Current state

- Message: "Ikaw at ikaw"
- Ribbon: baby blue
- Git history: one commit per round of changes, dated to when each version went live.

## Preview / testing

Playwright works for headless screenshots, e.g. open `file://…/index.html`, wait ~8 s for the
bloom, then screenshot. Check for page errors after any change.

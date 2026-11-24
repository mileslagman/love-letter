# love letter

An animated lily of the valley bouquet, drawn in colored text characters. It blooms in a lotus
pond under a blue moon, then types out a message: *Ikaw at ikaw*.

## Run it

Open `index.html` in any modern browser. There is nothing to install or build.

- **Drag** to turn the bouquet. After a few seconds alone, it turns slowly by itself.
- **Click or tap the strip under the message** to replay the bloom.

## How it was created

The page is plain HTML, CSS and JavaScript using the browser's Canvas 2D API. It has no
libraries, image files or web fonts. Every flower, leaf and petal is drawn by code.

It was built up in rounds, and the commit history follows them:

1. **First bouquet.** Stems grow, leaves unfold and bells open on a starry background, with a
   pink bow.
2. **More life.** More stems, light glowing inside each bell, a blue moon and a typed message.
3. **3D.** The flat bouquet became one you can drag and view from different angles.
4. **The pond.** The bouquet was rounded out on every side, the bow became baby blue, and lotus
   flowers and lily pads were added around it.
5. **The words.** The message changed a few times before settling on *Ikaw at ikaw*.

## How it works

### Turning shapes into characters

There are two canvases. For each frame:

1. The scene is painted with ordinary shapes and gradients onto a small hidden canvas, where
   **one pixel stands for one character**.
2. The page reads those pixels back.
3. Each pixel that isn't empty becomes a character on the visible canvas, in that pixel's color.

Brighter pixels get dense characters such as `@` and `#`, and dimmer ones get light characters
such as `.` and `:`. Some green areas use slashes and brackets to look like leaves. The choice of
character changes about five times a second, which makes the picture shimmer.

### The 3D

Stems, leaves, lotus petals and lily pads are positioned in 3D. `proj()` rotates each point for
the current view, then scales it by distance for perspective. The bells and bow are flat shapes
placed at 3D positions. Stems, leaves and lotuses are drawn from back to front so that nearer
ones cover those behind.

### The bouquet and pond

- **Stems** are Bézier curves made by `makeStem()`. There are 18 around the circle and one in the
  middle. Each carries a row of bells that gets smaller toward a bud at the tip.
- **Leaves** are eight tapered shapes, alternating tall and low.
- **Lotuses** have three rings of petals that open over time, then bob gently on the water.
- **Layout** comes from a seeded random number generator, so the bouquet and pond are arranged
  the same way on every load. The stars are placed randomly each time.

### The light

After the characters are drawn, a soft warm glow is added over each bell. The moon's blue halo is
drawn behind the characters. Both pulse slowly.

### The timeline

Everything follows one clock that starts when the page loads:

| Time | What happens |
| --- | --- |
| 0 s | The moon fades in, and the pond and the base of the stems begin to grow |
| about 0.6 s | Leaves start to unfold |
| about 1 s | Stems start to grow, and bells open along them |
| about 4.2 s | The bow pops in |
| about 5 s | The bouquet begins to sway |
| about 5.6 s | The message starts typing |

## Change it

The message is the `MESSAGE` constant near the top of the script in `index.html`. Long messages
shrink to fit the screen, down to a minimum size.

More technical notes are in [method.md](method.md).

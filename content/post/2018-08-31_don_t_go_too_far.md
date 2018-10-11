---
title     : "Don't Go Too Far"
date      : 2018-08-31T00:01:44+12:00
categories: ["post"]
tags      : ["development"]
---

{{< load-photoswipe >}}

*"Excuse me. Come back!"* said every game developer, ever. It's an all too familiar cry when a character runs out of screen into the vast empty space of nothingness. This update brings in map layers and boundaries, as well as a number of invisible improvements to speed up future development.

## Play

{{< youtube p99zjziGYCo >}}

Okay! So what's new? In order of appearance in the video:

* A (very basic) character selection screen.
* Animated map background.
* Map boundaries.
* XZ movement while jumping [as promised]({{ site.baseurl }}{% post_url 2018-07-20-now-we-re-running %}#play)
* Multiple players.

### Character Selection Screen

The character selection screen *works*, but only just. There's nothing that tells you that you:

1. Press attack to join.
2. Use left and right to select a character.
3. Press attack to confirm your selection.
4. Press attack *again* to start the game.

What's your attack button again? That's right, you won't know unless you open the controls file, which there isn't yet a screen for.

### Maps

Boundaries prevent characters from walking off the map into oblivion, or on air.

### XZ Movement

This deserves a mention as the underlying code was reorganized to allow for reuse across multiple sequences, and is substantially tested.

### Multiple Players

This is the beginning of *real gameplay*. What's missing is:

* **Additional sequences:** Animations for attacking and getting hit.
* **Collision detection:** Hit boxes and hurt boxes.
* **Character health:** Needed to determine a winning condition.

...and all of the polish on top of that such as hit sparks, delay, sound, and a scoreboard.

## Create

Map backgrounds and boundaries can now be defined, and there's a small breaking change to character sequences.

### Spriting

In the above clip, the background images in the *Eruption* map are created by Blue Phoenix and Alectric from the Little Fighter 2 world. These are single screen large images, but you can also create parts of the background with smaller images and combine them together.

{{< gallery caption-effect="fade" >}}
{{< figure src="/img/2018-08-31_don_t_go_too_far/ground.png" caption="Ground" >}}
{{< figure src="/img/2018-08-31_don_t_go_too_far/sky.png" caption="Sky" >}}
{{< /gallery >}}

### Scripting

#### Maps

Map backgrounds are done via sprites and sequences, and so they can be animated with different images. Right now you aren't able to specify coloured rectangle regions, but they are planned to be included, as a map with a background and a different coloured floor can already be visually appealing.

Images are specified using the existing sprite sheet format:

{{< highlight toml "linenos=table" >}}
# assets/default/map/eruption/sprites.toml
[[sheets]]
  # 0
  path = "ground.png"
  sprite_w = 800
  sprite_h = 199
  row_count = 3
  column_count = 1

[[sheets]]
  # 1
  path = "sky.png"
  sprite_w = 800
  sprite_h = 376
  row_count = 1
  column_count = 1
{{< /highlight >}}

The map boundaries and layers are defined in a separate file:

{{< highlight toml "linenos=table" >}}
# assets/default/map/eruption/map.toml
[header]
name   = "Eruption"
bounds = { x = 150, y = 0, z = 15, width = 450, height = 800, depth = 50 }

[[layer]]
# Sky
position = { x = 0, y = 134, z = 0 }
frames = [
  { sheet = 1, sprite = 0, wait = 10 },
]

[[layer]]
# Ground
position = { x = 0, y = 0, z = 1 }
frames = [
  { sheet = 0, sprite = 0, wait = 10 },
  { sheet = 0, sprite = 1, wait = 10 },
  { sheet = 0, sprite = 2, wait = 10 },
]
{{< /highlight >}}

The syntax for declaring layers should look familiar &mdash; it also uses the animation code used by character sequences. The order of the `[[layer]]`s affects the order that the layers are drawn, but it is also better to control this with the `z` coordinate.

This post doesn't explain the configuration in detail, but in the far future there are plans to have complete tutorials and explanations for everything.

#### Characters

The `stop_run` sequence has been renamed to `run_stop`. This makes it consistent with the `jump` sequences which are all `jump_*`, so it is easier to identify a group of related sequences.

{{< highlight toml "linenos=table" >}}
[sequences.run]
  # ...
[sequences.run_stop] # previously `sequences.stop_run`
  frames = [{ sheet = 0, sprite = 11, wait = 3 }]
{{< /highlight >}}

## What's Next

Previously we ran. Right now we're bound. Hereafter we fight.

Look out for hit and hurt boxes, collision, and winning.

That's where the fun begins.

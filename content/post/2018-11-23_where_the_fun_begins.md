---
title     : "Where The Fun Begins"
date      : 2018-11-23T06:01:13+13:00
categories: ["post"]
tags      : ["development"]
---

> When your face aligned with my fist, did you feel the connection? :punch:

Collision has arrived! Right now only cuboid-cuboid intersection is implemented, but it demonstrates the concept.

Let's have a look:

{{< youtube q0YSfNZSNqU >}}

## Play

What's new:

* Collision detection and reaction.
* Character health points (HP).
* Winning condition &ndash; last person standing wins.

Finally we can *play*.

## Create

As with every new feature, we need configuration and artwork to make it look good.

When an object is hit, we need to give it a hurt animation. For characters, there are 6 new sequences:

* `flinch_0`: Sequence when hit.
* `flinch_1`: Alternative sequence when hit.
* `fall_forward_ascend`: Falling, but vertical movement is upwards.
* `fall_forward_descend`: Falling, vertical movement is downwards.
* `fall_forward_land`: Character has hit the ground.
* `lie_face_down`: Character is lying on the ground.

The following sprite sheet shows the sprites used in these sequences:

{{< figure src="/img/2018-11-23_where_the_fun_begins/heat_interaction.png" caption="heat_interaction.png" alt="Sprites used when a character is hit." >}}

The corresponding object configuration is as follows:

{{< highlight toml "linenos=table" >}}
# Flinch when character is hit.
#
# * `flinch_0`: Got hit when on ground.
# * `flinch_1`: Got hit when on ground - alternate sequence.
[sequences.flinch_0]
  frames = [
    { sheet = 3, sprite = 24, wait = 2, body = [ .. ] },
    { sheet = 3, sprite = 25, wait = 2, body = [ .. ] },
    { sheet = 3, sprite = 24, wait = 2, body = [ .. ] },
  ]

[sequences.flinch_1]
  frames = [
    { sheet = 3, sprite = 26, wait = 2, body = [ .. ] },
    { sheet = 3, sprite = 27, wait = 2, body = [ .. ] },
    { sheet = 3, sprite = 26, wait = 2, body = [ .. ] },
  ]

# Fall when knocked down.
[sequences.fall_forward_ascend]
  frames = [
    { sheet = 3, sprite = 6, wait = 2 },
  ]

[sequences.fall_forward_descend]
  frames = [
    { sheet = 3, sprite = 7, wait = 3 },
    { sheet = 3, sprite = 8, wait = 4 },
    { sheet = 3, sprite = 9, wait = 2 },
  ]

[sequences.fall_forward_land]
  frames = [
    { sheet = 3, sprite = 10, wait = 2 },
    { sheet = 3, sprite = 11, wait = 2 },
  ]

[sequences.lie_face_down]
  frames = [
    { sheet = 3, sprite = 10, wait = 30 },
  ]
{{< /highlight >}}

## What's Next

This update doesn't have that many feature changes, and this will be the case for the next one or two updates &ndash; there is a lot of maintenance and behind-the-scenes work to allow for faster development in the future. Planned features include:

* **Collision shapes:** Collision zones defined as spheres, cylinders, and other shapes.
* **Dynamic interaction:** Characters become dizzy after being hit a number of times.
* **Additional sequences:** Jump while running, attack while jumping, etcetera.
* **Better Menu:** The current one doesn't show what state it's in, or what to do.

*But*, before we can get to that, these things must be done:

* Reorganize runtime data and logic.
* Rewrite asset loading.
* Use physics library for optimized collision detection.
* Automate collision tests.

The fun is just beginning; and it's a long way to the end.

Peace :v:!

---
title     : "This Is Lit"
date      : 2019-07-05T22:33:14+13:00
categories: ["post"]
tags      : ["development", "release"]
---

> Don't play with fire 🔥

This update adds support for spawning energy objects:

{{< youtube 992OirOjA_0 >}}

That is the only *apparent* change; other significant changes include:

* Rendering has changed from OpenGL to Vulkan.
* Early support for teams.
* Object deletion at the end of a sequence.

The new version can be downloaded on [itch.io](https://azriel91.itch.io/will).

## Play

### Energy Spawning

{{< figure src="/media/2019-07-05_this_is_lit/spawn_energy.png" caption="Energy spawning" >}}

Energy objects are now spawnable in game, expanding game play possibilities. This is one step towards enabling:

* Actions based on a sequence of control buttons.
* Charged shots.

## Create

### Configuration

#### Object Deletion Via `next`

Previously, Will accepted a sequence ID to switch to when a sequence ended, or the application would choose a default.

Now, Will still accepts a sequence ID, but it also accepts the following values:

* `none`: If there is a default sequence transition, use that. Otherwise, remain on the last frame.
* `repeat`: Repeat the current sequence.
* `delete`: Delete the current object.

{{< highlight toml "linenos=table" >}}
[sequences.hover]
  next = "" # defaults to "none"
  frames = [
    # ..
  ]
{{< /highlight >}}

This change was made to allow the fireballs to disappear after they hit or are hit by another object.

#### New Type: Energy

There is a new type -- energy. This is used to represent energy blasts and visual effects.

Right now it has 3 sequences:

* `hover`: Default sequence when the energy is hovering.
* `hit`: Sequence the energy switches to when it is hit by another object.
* `hitting`: Sequence the energy switches to when it hits another object.

The following shows the object definition used for the fireball object in the video:

{{< highlight toml "linenos=table" >}}
[sequences.hover]
  [[sequences.hover.frames]]
    wait = 4
    sprite = { sheet = 0, index = 0 }
    body = [{ box = { x = 15, y = 6, w = 39, h = 21 } }]

    [[sequences.hover.frames.interactions]]
      hit = { repeat_delay = 20, hp_damage = 15, stun = 90 }
      bounds = [{ box = { x = 15, y = 6, w = 39, h = 21 } }]

  [[sequences.hover.frames]]
    wait = 4
    sprite = { sheet = 0, index = 1 }
    body = [{ box = { x = 15, y = 6, w = 39, h = 21 } }]

    [[sequences.hover.frames.interactions]]
      hit = { repeat_delay = 20, hp_damage = 15, stun = 90 }
      bounds = [{ box = { x = 15, y = 6, w = 39, h = 21 } }]

[sequences.hit]
  next = "delete"
  frames = [
    { wait = 1, sprite = { sheet = 0, index = 2 } },
    { wait = 1, sprite = { sheet = 0, index = 3 } },
    { wait = 1, sprite = { sheet = 0, index = 4 } },
    { wait = 1, sprite = { sheet = 0, index = 5 } },
  ]

[sequences.hitting]
  next = "delete"
  frames = [
    { wait = 1, sprite = { sheet = 0, index = 2 } },
    { wait = 1, sprite = { sheet = 0, index = 3 } },
    { wait = 1, sprite = { sheet = 0, index = 4 } },
    { wait = 1, sprite = { sheet = 0, index = 5 } },
  ]
{{< /highlight >}}

#### Spawns

Energy objects are spawned using the `spawns` frame element. The position and velocity of the spawned energy can be specified, which is be relative to the position and velocity of the entity that spawns it.

The following shows an example of a `spawns` frame element:

{{< highlight toml "linenos=table" >}}
[sequences.stand_attack_1]
  [[sequences.stand_attack_1.frames]]
    # ..

    [[sequences.stand_attack_1.frames.spawns]]
      object = "default/fireball" # Slug of the energy object to spawn
      position = [45, 35, 0]      # Position relative to the sprite's top left corner
      velocity = [6, 0, 0]        # Velocity that the object begins with.
{{< /highlight >}}

Multiple `spawns` may be specified to spawn multiple objects.

### Spriting

{{< figure src="/media/2019-07-05_this_is_lit/fireball.png" caption="Fireball" >}}

Energy objects, like characters, need multiple sprites to form an aesthetic animation. The spritesheet above shows two sprites used during the `hover` sequence, and 4 sprites used during the `hit` and `hitting` sequences.

## What's Next

A significant portion of the next period will be dedicated to integrating the [`atelier-assets`](https://github.com/amethyst/atelier-assets) library into Amethyst, so there may not be very much feature development in Will. That said, I shall try to fit something in.

## Support Me

The game is still in an early stage, but if you would like to support me, please consider becoming a [Patron](https://www.patreon.com/azriel91).

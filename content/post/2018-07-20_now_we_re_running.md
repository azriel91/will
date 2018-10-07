---
title     : "Now We're Running"
date      : 2018-07-20T08:52:37+12:00
categories: ["Post"]
tags      : ["Development"]
---

Depending on the situation, you may or may not want to hear, "It moved. It's alive!" In this case, it's a good thing. Characters now can walk, run, and jump.

## Play

{{< figure src="/img/2018-07-20_now_we_re_running/walk_run.gif" >}}

Press left / right to walk; or double tap and hold to run. When stopping, there is a little delay before the character returns to the standing sequence.

{{< figure src="/img/2018-07-20_now_we_re_running/jump.gif" >}}

Press jump to jump. Currently there is no movement in the X or Z directions; this will be included in the next update.

## Create

Characters have 2 new sequences for running, and 5 new sequences for jumping.

By double tapping left or right, the character switches to the `run` sequence. Running loops back to the beginning of the sequence while the player holds left / right in the direction of the run. When the player releases the key, or presses both left and right at the same time, the character switches to the `stop_run` sequence and subsequently back to `stand`.

When the player presses jump, the character goes through multiple sequences:

1. `jump` as the preparation sequence.
2. `jump_off` when the character first lifts off the ground.
3. `jump_ascend` while the character's velocity is positive upwards.
4. `jump_descend` when the character's velocity switches to downwards.
5. `jump_descend_land` when the character lands on the ground.

This allows the jump to look more natural, as well as allow for better scripting control in each part of the jump.

### Spriting

For the new sequences, the following set of sprites are enough to allow for fluid animation:

{{< figure src="/img/2018-07-20_now_we_re_running/heat_run_jump.png" >}}

The first 3 sprites are used for running, and the 4th is used when the character stops.

On the bottom row, the first two are used when the character begins to jump, as well as when it lands. The third is used while it is in the air.

The sprite poses aren't limited to this &mdash; you can also have different sprites when the character is moving upwards and downwards, as well as animate the clothes for a wind motion effect.

### Scripting

With the above sprites, the following shows how you can script the running and jumping sequences:

{{< highlight toml "linenos=table" >}}
# Loops while the user presses left or right.
[sequences.run]
  frames = [
    { sheet = 0, sprite =  8, wait = 2 },
    { sheet = 0, sprite =  9, wait = 2 },
    { sheet = 0, sprite = 10, wait = 2 },
    { sheet = 0, sprite =  9, wait = 2 },
  ]

[sequences.stop_run]
  frames = [{ sheet = 0, sprite = 11, wait = 3 }]
{{< /highlight >}}

There are 5 sequences for jumping to look natural.

{{< highlight toml "linenos=table" >}}
# The initial "getting ready to jump" sequence.
[sequences.jump]
  frames = [
    { sheet = 0, sprite = 12, wait = 2 },
    { sheet = 0, sprite = 13, wait = 1 },
  ]

# Liftoff! This is separate from `jump_ascend` so that
# you can add in a "dust effect".
[sequences.jump_off]
  frames = [{ sheet = 0, sprite = 14, wait = 1 }]

[sequences.jump_ascend]
  frames = [{ sheet = 0, sprite = 14, wait = 1 }]

# This sequence is used when the character starts moving
# downwards.
[sequences.jump_descend]
  frames = [{ sheet = 0, sprite = 14, wait = 1 }]

[sequences.jump_descend_land]
  frames = [
    { sheet = 0, sprite = 12, wait = 0 },
    { sheet = 0, sprite = 13, wait = 0 },
    { sheet = 0, sprite = 12, wait = 1 },
  ]
{{< /highlight >}}

## What's Next

Next time there should be improvements to character movement and a rendered map (background). If there is enough time, I'll also implement a basic character selection screen.

Now we're running.

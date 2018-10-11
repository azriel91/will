---
title     :  "Take Control"
date      :  2018-06-08T21:13:52+12:00
categories: ["post"]
tags      : ["development"]
---

This update adds support for controller input and sequence switching for characters. It also changes the syntax in object configuration for declaring sequences. It's a small update as I had just under 3 weeks to work on this.

## Play

{{< figure src="/img/2018-06-08_take_control/control_input.gif" caption="Characters now transition from standing to walking upon controller input. This includes movement on both the X axis and Z axis." >}}

*That's it?*  
Yeap!

*Wait, that's how you answered last time.*  
It was, but it takes quite a lot of work to do small things.

Right now, the controller configuration looks like this:

```rust
// For a list of key codes, see <https://docs.rs/winit/0.13.1/winit/enum.VirtualKeyCode.html>
(
    axes: {
        // Players are numbered using zero based indices, so player "one" is 0
        (player: 0, axis: X): (pos: Key(Right), neg: Key(Left)),
        (player: 0, axis: Z): (pos: Key(Down), neg: Key(Up)),
    },
    actions: {
        (player: 0, action: Defend): [Key(O)],
        (player: 0, action: Jump): [Key(P)],
        (player: 0, action: Attack): [Key(LBracket)],
        (player: 0, action: Special): [Key(RBracket)],
    },
)
```

It is structured so that more controllers can be defined, though currently only for keyboard controllers. You can also see that there are four action buttons. Those who have played Little Fighter 2 or Hero Fighter will find these familiar.

## Create

### Scripting

Character sequences have now changed. Previously sequences were written like this:

{{< highlight toml "linenos=table" >}}
[[sequences]]
  id = "Standing"
  next = "Standing"
  frames = [
    # ...
  ]

[[sequences]]
  id = "Walking"
  next = "Standing"
  frames = [
    # ...
  ]
{{< /highlight >}}

In this update, the sequence ID is now lowercase<sup>1</sup>, no longer ends with "-ing", and are put into single square brackets:

{{< highlight toml "linenos=table" >}}
[sequences.stand]
  next = "stand"
  frames = [
    # ...
  ]

[sequences.walk]
  next = "stand"
  frames = [
    # ...
  ]
{{< /highlight >}}

<sup>1</sup> <sub>Strictly speaking, the game uses *snake_case*. This is a programming term to describe lower cased words separated by underscores "`_`". Since there are currently just single word IDs, it just looks like lowercase.</sub>

## What's Next

So the "reading controller input" half is done, next time we should be able to move the character around.

Onwards!

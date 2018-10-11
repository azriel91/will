---
title     : "Humble Beginnings"
date      : 2018-04-27T08:34:54+12:00
categories: ["post"]
tags      : ["development"]
---

{{< load-photoswipe >}}

What does it look like, when something begins with nothing? It's exactly how you imagine it &mdash; nothing. Then something takes shape. It's been 12 weeks since I began development on Will. Here's a look at where it's at.

## Play

{{< figure src="/img/2018-04-27_humble_beginnings/preview.gif" caption="Static menu and animated sprites" >}}

Right now, Will is simply a menu that clicks through to an animated sprite sequence.

*That's it?*  
Yeap!

*Wait, what about that red guy?*  
Read on to content creation!

## Create

### Spriting

For objects to fit together, the artwork should not only look good, but also be consistent &mdash; looks like "the same person drew it".

So, about this guy:

{{< figure src="/img/2018-04-27_humble_beginnings/sprite.gif" alt="Example character &mdash; Heat &mdash; standing next to a bat." >}}
{{< figure src="/img/2018-04-27_humble_beginnings/heat_standing.png" alt="Sprites for Heat's standing animation." >}}

The sprites for this character, Heat, were drawn by Apocalipsis, for one of my previous attempts at making this game. Heat may or may not be part of the final game, as I haven't settled on the art style for Will.

### Scripting

Scripting is not just for "the coders"; sprite animations need scripts too. The first part is to tell the game where an object's sprites are, and the offset of the sprite from where the object should be. This will be explained in a later post.

{{< highlight toml "linenos=table" >}}
[[sheets]] # 0
  path = "heat_standing.png"
  sprite_w = 79.0
  sprite_h = 79.0
  row_count = 1
  column_count = 4
  offsets = [
    { x =  35, y = 79 }, # 0
    { x = 115, y = 79 }, # 1
    { x = 195, y = 79 }, # 2
    { x = 275, y = 79 }, # 3
  ]
# ...
{{< /highlight >}}

The next part is to animate the sprites. In Will, animations and object interaction are defined together &mdash; a punch animation and the attack zone change together. Objects cannot interact with each other yet, so the following *object definition* only includes the animation part.

{{< highlight toml "linenos=table" >}}
[[sequences]]
  id = "Standing"
  next = "Standing"
  frames = [
    { sheet = 0, sprite = 0, wait = 2 },
    { sheet = 0, sprite = 1, wait = 2 },
    { sheet = 0, sprite = 2, wait = 2 },
    { sheet = 0, sprite = 3, wait = 2 },
  ]
{{< /highlight >}}

These are (simple) examples of what the object scripts will look like. Later on, there will be more things to write in the object definition, and explanations on what each part means.

## What's Next

The next feature for Will is reading controller input and moving a character around. It may not come in the next update, but in the update after that as there is plenty to do for that to work properly.

Until then!

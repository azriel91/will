---
title     : "Charging Up"
date      : 2019-08-16T09:21:26+13:00
categories: ["post"]
tags      : ["development", "release"]
---

> 🔋 We're in for a long ride. Make sure you're charged up.

In this update, characters can now charge up by holding the `Attack` button:

{{< youtube k2ln_qY9uP4 >}}

Behind the scenes, (most) sequence transitions based on control input have been made configurable.

The new version can be downloaded on [itch.io](https://azriel91.itch.io/will).

## Play

### Charging

<div>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/charge_0.png" caption="Charging" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/charge_1.png" caption="Buster 1" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/charge_2.png" caption="Buster 2" >}}</span>
</div>

Characters can now execute different attacks based on how many *charge points* they have. How charge points are used differs depending on the character:

* A Rockman / Megaman style character will use up all charge points when unleashing an energy blast.
* A fantasy hero's attacks are stronger with charge, but charge points decrease over time.
* A gun wielder's charge represents their ammo, which doesn't fade, but is limited.

Currently, charge points increase while the `Attack` button is held. In the future, other methods of charging may be implemented to allow other gameplay mechanics:

* **2D Fighter:** Charge when an attack lands.
* **Proximity:** Charge when in a particular area.

## Create

### Configuration

#### Charging

As there are many different types of charge up moves, there are also many configuration flags to adjust the character's actions. The following snippet explains the configuration:

{{< highlight toml "linenos=table" >}}
# object.toml

# Max charge points to store.
charge_limit = 50

# Number of ticks to wait per charge point.
#
# Will runs at 60 FPS, setting this to 1 means 60 charge points per second.
charge_delay = 1

# How many charge points to subtract when used.
#
# Possible values:
#
# * `"exact"`:
#
#    The exact number of charge points are spent.
#
# * `"nearest_partial"`:
#
#    Subtract to the nearest multiple specified, an incomplete multiple counts.
#    If the entity has 21 to 30 `ChargePoints`, and the charge cost is 10, it will drop to 20.
#
# * `"nearest_whole"`:
#
#    Subtract to the nearest multiple specified, an incomplete multiple does not count.
#    If the entity has 20 to 29 `ChargePoints`, and the charge cost is 10, it will drop to 10.
#
# * `"all"`:
#
#    All charge points are spent, regardless of charge cost.
#
charge_use_mode = "all"

# How charge is retained when no longer charging.
#
# * `"forever"`: Charge does not decrease unless used.
# * `"never"`: Charge is always reset to 0.
# * `{ lossy = { delay = 10 } }`: One charge point is lost every 10 ticks.
# * `{ reset = { delay = 60 } }`: Charge points are reset to 0 after 60 ticks without charging.
charge_retention_mode = "never"

[sequences.stand]
  [sequences.stand.transitions.press_attack]
    # Buster shot - weak
    next = "stand_attack_0"

  [[sequences.stand.transitions.release_attack]]
    # Buster shot - strong
    #
    # Hijacking the `dazed` sequence while arbitrary sequence names is not yet implemented.
    next = "dazed"
    requirements = [{ charge = 50 }]

  [[sequences.stand.transitions.release_attack]]
    # Buster shot - mid
    next = "stand_attack_1"
    requirements = [{ charge = 20 }]

  [[sequences.stand.frames]]
    wait = 8
    sprite = { sheet = 0, index = 0 }
    body = [{ box = { x = 16, y = 10, w = 28, h = 53 } }]

{{< /highlight >}}

### Spriting

<div>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/rox_outline.gif" caption="Outline" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/rox_colour.gif" caption="Colour" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/rox_shaded.gif" caption="Shaded" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/buster_shot_weak.gif" caption="Buster Shot Weak" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/buster_shot_mid.gif" caption="Buster Shot Mid" >}}</span>
<span style="display: inline-block;">{{< figure src="/img/2019-08-16_charging_up/buster_shot_strong.gif" caption="Buster Shot Strong" >}}</span>
</div>

There aren't any new sequence IDs in this update, but it's still fun to create new moves. The above character sprites and buster shots were made using [aseprite](https://www.aseprite.org/). It costs US $15, but it's well worth it.

## What's Next

As mentioned, a significant amount of the 6 weeks were spent on open source projects ([`amethyst`](https://github.com/amethyst/amethyst), [`atelier-assets`](https://github.com/amethyst/atelier-assets)), though that value has not yet fully been realized. This will continue into the next development period, but I will still aim to implement the following features:

* Arbitrary sequence IDs.
* Kinematics in configuration.

## Support Me

The game is still in an early stage, but if you would like to support me, please consider becoming a [Patron](https://www.patreon.com/azriel91).

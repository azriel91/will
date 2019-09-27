---
title     : "Into The Wild"
date      : 2019-05-24T17:33:58+13:00
categories: ["post"]
tags      : ["development", "release"]
---

> ⚙️ An automation engineer writes a game 👾
>
> 🎮 Nobody plays it.

**PSA:** *This update is the first with a [public download](https://azriel91.itch.io/will). Currently it's pay what you want -- there is minimal content, but this will grow over the year. I have also begun a [Patreon](https://www.patreon.com/azriel91).*

In the past eight weeks, we've added sound, hit interaction mechanics, control transitions, and UI improvements.

Let's have a look:

{{< youtube 9yx6fC1YtV8 >}}

Here's the list of notable visible (and audible!) changes:

* UI includes instructions on how to navigate.
* UI sounds.
* Speed of the `jump` and `dash` sequences have been reduced.
* When a character hits another, a sound is played.
* When a character hits another, there is a small pause or *freeze frame*.
* HP bars are in the right place.

Alongside that is the list of less visible changes. More detail is further in the post.

* Hit mechanics.
* Stun points.
* Configurable control transitions.
* Added an end user license agreement (EULA).
* Ship with default assets.

## Play

### Hit Mechanics

The logic to determine when an attack hits a character is guarded by two mechanisms:

* **Hit Limit**

    The number of objects the attack may hit. This can be limited to:

    - **A single object:** For a simple attack such as a punch.
    - **A fixed number of objects:** For a medium-strength attack, like a limited area-of-effect spell.
    - **Unlimited number of objects:** For a strong attack, like an explosion.

* **Hit Repeat Delay** (or *cooldown*[^1])

    Number of *ticks* that must pass before an attacker may hit the same object. In a 60 FPS game, if a character has an ongoing attack with a hit repeat delay of 10, it would hit the same object 6 times.

    The hit repeat delay is tracked separately for each object that is hit.

### Stun

Attacks can inflict stun points. When a character accumulates enough stun points, they go into the `dazed` sequence, where they cannot fend against further attacks. If a character is further attacked, they will fall even if they still have health points.

Stun points reduce by 1 point per tick.

### Control Transitions

Characters can now perform different moves (sequences) depending on whether you press, hold, or release an action key. This enables and distinguishes the following types of moves:

* **Regular moves:** Press attack to punch, jump to jump.
* **Charged shot:** Hold attack to charge, release to fire like Rockman / Megaman.
* **Lasers:** Hold attack to continuously fire.
* **Defend:** Hold defend to continuously guard.

## Create

There are 4 new sequences, and one renamed sequence:

* `stand_attack` has been renamed to `stand_attack_0`.
* `stand_attack_1` is a second attack sequence.
* `jump_attack` is added for attacking while jumping.
* `dash_attack` is added for attacking while dashing.
* `dazed` is used when a character is hit and has accumulated enough stun points.

### Configuration

#### Interaction

The way `interactions` are specified has changed to accomodate the new hit mechanics. For a normal hit, the following snippet will tend to do want you expect:

{{< highlight toml "linenos=table" >}}
[[sequences.stand_attack_0.frames.interactions]]
  hit = { hp_damage = 5 }
  bounds = [{ box = { x = 50, y = 32, w = 17, h = 18 } }]
{{< /highlight >}}

The following shows all of the values that can be specified for a hit interaction:

{{< highlight toml "linenos=table" >}}
[[sequences.stand_attack_0.frames.interactions]]
  bounds = [{ box = { x = 50, y = 32, w = 17, h = 18 } }]

  [sequences.stand_attack_0.frames.interactions.hit]
    # Default: 1. May also be "unlimited".
    hit_limit = 3

    # Default: 10 ticks.
    repeat_delay = 20

    # Default: 0 health points.
    hp_damage = 5

    # Default:  0 skill points.
    sp_damage = 5

    # Default: 30 stun points.
    #
    # When a character is hit, transitions to the
    # following sequences if sufficient stun points:
    #
    # Below  40:   `flinch_0`
    # Below  80:   `flinch_1`
    # Below 120:   `dazed`
    # 120 or more: `fall_forward_ascend`
    stun = 50
{{< /highlight >}}

#### Control Transitions

In character configuration, . The following snippet of a frame in the `stand` sequence shows the configuration to transition to `stand_attack_0` when `Attack` is pressed, or `jump` when `Jump` is pressed:

{{< highlight toml "linenos=table" >}}
[sequences.stand]
  [[sequences.stand.frames]]
    # ..

    [[sequences.stand.frames.transitions]]
      press_attack = "stand_attack_0"
      press_jump = "jump"
{{< /highlight >}}

For *hold* and *release* transitions, the `hold_<button>` and `release_<button>` fields are used to specify what sequence to switch to when the button is held or released on that frame:

{{< highlight toml "linenos=table" >}}
[sequences.stand]
  [[sequences.stand.frames]]
    # ..

    [[sequences.stand.frames.transitions]]
      press_attack = "stand_attack_0"
      press_jump = "jump"
      hold_defend = ".."    # defend -- to be done.
      release_attack = ".." # charge_shot -- to be done.
{{< /highlight >}}

Currently, because control transitions are specified per *frame*, if you have 4 standing frames, you have to copy and paste the transitions to all frames. In a later version, common transitions can be specified at the sequence level.

### Spriting

{{< figure src="/media/2019-05-24_into_the_wild/heat_stand_attack.png" caption="Stand attack" >}}  
{{< figure src="/media/2019-05-24_into_the_wild/heat_jump_attack.png" caption="Jump attack" >}}  
{{< figure src="/media/2019-05-24_into_the_wild/heat_dash_attack.png" caption="Dash attack" >}}  
{{< figure src="/media/2019-05-24_into_the_wild/heat_dazed.png" caption="Dazed" >}}  

There are now two sequences for stand attack to make the movement appear more natural, and characters can now attack while jumping and dashing.

Finally, the `dazed` sequence is when the character has been hit multiple times, and is *dazed* from the attack.

## What's Next

The next release will be a maintenance release to allow arbitrarily named sequences to be used in configuration. I'll also try to work on the ability to spawn objects in game, and specifying acceleration values in object configuration.

## Support Me

The game is still in an early stage, but if you would like to support me, please consider becoming a [Patreon](https://www.patreon.com/azriel91).

[^1]: The word "cooldown" will likely be used in the future for skill execution delays.

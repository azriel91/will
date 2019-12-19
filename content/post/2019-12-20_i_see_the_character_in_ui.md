---
title     : "I See The Character In UI"
date      : 2019-12-20T00:36:01+13:00
categories: ["post"]
tags      : ["development", "release"]
---

> 🤨 How did you come to be random?
>
> ☑️ I had too many choices.

This update continues the work on the user interface, with a configuration defined character selection UI, and the beginning of the control settings UI.

{{< youtube aQiK5rOylCY >}}

The new version can be downloaded on [itch.io](https://azriel91.itch.io/will).

What's new:

* Character selection UI displays the character that the player will use.
* Control settings UI allows players to view the configured control keys.
* User interfaces (UIs) are largely defined through configuration, making development and customization easier.
* Events to control application behaviour can be defined in configuration.

What's missing ([post]({{< ref "/post/2019-11-08_that_looks_good_on_ui.md" >}}#what-s-next)):

* Map selection UI is not yet improved.
* Scoreboard is not yet improved.

## Play

### Character Selection UI

{{< gallery hover-effect="none" caption-effect="fade" >}}
{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/character_selection_ui.png" caption="Character Selection UI" >}}
{{< /gallery >}}

The character selection UI displays the character that the player will use (and it's animated!), with colour[^1] to indicate whether the selection is confirmed. This is simply the first visual version -- further improvements shall be made in the future.

### Control Settings UI

{{< gallery hover-effect="none" caption-effect="fade" >}}
{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/control_settings_ui.png" caption="Control Settings UI" >}}
{{< /gallery >}}

The control settings UI displays a US keyboard on screen, whose keys react to key presses. In this version, the control buttons cannot yet be changed in the screen, and any `Attack` button press will return to the game mode selection menu.

Many thanks to Kadith ([kadiths-free-icons](https://kadith.itch.io/kadiths-free-icons)) for making the sprites used for the keys 🙇!

## Create

### Spriting

<div>
<span style="display: inline-block;">{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/rox_breathe.gif" caption="Breathing animation for Rox." >}}</span>
<span style="display: inline-block;">{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/rox_flinch.gif" caption="Flinch animation for Rox." >}}</span>
<span style="display: inline-block;">{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/rox_fall_back.gif" caption="Falling animation for Rox." >}}</span>
</div>

Rox has now got animations for the `stand`, `flinch_*`, and `fall_*` sequences. The flinch sequence doesn't really look its part, but at least there is visual feedback when he gets hit.

### Configuration

The following user interfaces are (now) mostly defined by configuration:

* Game mode selection
* Character selection
* Control settings

For the curious, feel free to edit the various YAML files in the `assets/default/ui/*` directories. The following example shows how to change the pulse colour when the character selection is confirmed, from green to purple:

<div>
<span style="display: inline-block;">{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/pulse_green.gif" caption="Breathing animation for Rox." >}}</span>
<span style="display: inline-block;">{{< figure src="/media/2019-12-20_i_see_the_character_in_ui/pulse_purple.gif" caption="Flinch animation for Rox." >}}</span>
</div>

Change the tint from this:

{{< highlight yaml "linenos=table" >}}
---
# character_selection/ui.yaml
# ..

sequences:
  # ..

  widget_confirmed:
    # ..
    frames:
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 0.6, g: 2.1, b: 1.0 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 0.65, g: 2.3, b: 1.0 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 0.7, g: 2.5, b: 1.0 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 0.75, g: 2.7, b: 1.0 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 0.7, g: 2.5, b: 1.0 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 0.65, g: 2.3, b: 1.0 } }
{{< /highlight >}}

To this:

{{< highlight yaml "linenos=table" >}}
---
sequences:
  # ..

  widget_confirmed:
    # ..
    frames:
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 3.3, g: 1.1, b: 2.1 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 3.5, g: 1.3, b: 2.3 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 3.7, g: 1.5, b: 2.5 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 3.9, g: 1.7, b: 2.7 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 3.7, g: 1.5, b: 2.5 } }
      - { wait: 3, sprite: { sheet: 1, index: 0 }, tint: { r: 3.5, g: 1.3, b: 2.3 } }
{{< /highlight >}}

## What's Next

The next update will still focus on the user interface -- improving the map selection UI and game ending score board that were missed this time -- as well as ironing out the ones introduced this release.

The next development period will be shorter, but shall still release the next version in 6 weeks.

Until then 👋.

## Support Me

The game is still in development; if you would like to support me, please consider becoming a [Patron](https://www.patreon.com/azriel91).

[^1]: Configuration is not colour-blind friendly in this version.

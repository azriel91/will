---
title     : "I Choose UI"
date      : 2020-01-31T00:13:01+13:00
categories: ["post"]
tags      : ["development", "release"]
---

> 🔭 Amidst all the possibilities I see you 🌠

Once again, the user interface (UI) is improved. Now the available characters and maps are shown during selection, and players' control buttons are displayed on each screen.

{{< youtube z2KG_prMJ1c >}}

The new version can be downloaded on [itch.io](https://azriel91.itch.io/will).

What's new:

* Character selection UI lists all available characters, with player selections highlighted.
* Map selection UI lists all available maps, with a selection highlight.

What's missing ([post]({{< ref "/post/2019-12-20_i_see_the_character_in_ui.md" >}}#what-s-next)):

* Scoreboard is not yet improved.

## Play

### Character and Map Selection UI

{{< gallery hover-effect="none" caption-effect="fade" >}}
{{< figure src="/media/2020-01-31_i_choose_ui/character_selection_ui.png" caption="Character Selection UI" >}}
{{< figure src="/media/2020-01-31_i_choose_ui/map_selection_ui.png" caption="Map Selection UI" >}}
{{< /gallery >}}

Both the character and map selection UIs now list all available characters and maps, and the selected character or map is highlighted.

### Control Buttons

<div>
<span style="display: inline-block;">{{< figure src="/media/2020-01-31_i_choose_ui/control_buttons.gif" caption="Control button display." >}}</span>
</div>

Each player's control buttons are now displayed on screen, making it easier to figure out what to press. There are not yet labels for what button each key corresponds to -- the sprites are not yet made -- but it's an improvement I'd like to have.

Once again, many thanks to Kadith ([kadiths-free-icons](https://kadith.itch.io/kadiths-free-icons)) for making the sprites used for the keys 🙇!

## Create

<div>
<span style="display: inline-block;">{{< figure src="/media/2020-01-31_i_choose_ui/character_select.gif" caption="Character selection highlight" >}}</span>
<span style="display: inline-block;">{{< figure src="/media/2020-01-31_i_choose_ui/map_select.gif" caption="Map selection highlight" >}}</span>
</div>

The selection highlight for both character and map selection are defined in the usual way with sprites and sequence configuration. The following sections explain how the above highlight is created.

### Character Selection Highlight

<div>
<span style="display: inline-block;">{{< figure src="/media/2020-01-31_i_choose_ui/player_tags.png" caption="Character selection highlight sprites" >}}</span>
</div>

The selection highlight for character selection is simply a sprite per controller. Each highlight uses 3 sequences:

* `pX_inactive`: When the player has not joined.
* `pX_select`: When the player is selecting a character.
* `pX_confirmed`: When the player has confirmed selection.

The configuration for the sequences is as follows:

{{< highlight yaml "linenos=table" >}}
---
# assets/default/ui/character_selection/ui.yaml
sequences:
  p0_inactive:
    input_reactions:
      press_attack: { next: "p0_select",   events: [{ asset_selection: "join"   }] }
      press_jump:   { next: "p0_inactive", events: [{ asset_selection: "return" }] }
    frames: [{ wait: 2, sprite: { sheet: 4, index: 0 } }] # Blank sprite

  p0_select:
    input_reactions:
      press_attack: { next: "p0_confirmed", events: [{ asset_selection: "select" }] }
      press_jump:   { next: "p0_inactive",  events: [{ asset_selection: "leave"  }] }
      press_x:
        - { next: "p0_select", events: [{ asset_selection: { switch: "previous" } }], requirement: [{ input_dir_x: "left"  }] }
        - { next: "p0_select", events: [{ asset_selection: { switch: "next"     } }], requirement: [{ input_dir_x: "right" }] }
    frames: [{ wait: 2, sprite: { sheet: 5, index: 0 }, scale: 1.0 }]

  p0_confirmed:
    next: "repeat"
    input_reactions:
      press_attack: { next: "p0_confirmed", events: [{ asset_selection: "confirm"  }] }
      press_jump:   { next: "p0_select",    events: [{ asset_selection: "deselect" }] }
    frames: [{ wait: 2, sprite: { sheet: 5, index: 0 }, scale: 2.0 }]
{{< /highlight >}}

Each sequence indicates the sequence to switch to upon input, as well as the events to send. The `"return"` and `"confirm"` events have special handling in code to be sent only when conditions are met -- `"return"` only takes effect when there are no active players, and `"confirm"` requires that there are at least two players that are joined, and all have confirmed their character selections.

### Map Selection Highlight

<div>
<span style="display: inline-block;">{{< figure src="/media/2020-01-31_i_choose_ui/map_highlight_squiggle.gif" caption="Map selection highlight squiggle." >}}</span>
</div>

The selection highlight for map selection is a squiggle that moves around a square, repeated on all 4 sides. The animation above shows one of these squiggles, and colour is applied through sequence configuration. The selection highlight uses 2 sequences:

* `highlight_select`: When the map is being selected.
* `highlight_confirmed`: When the map selection is confirmed.

The configuration for the sequences is as follows:

{{< highlight yaml "linenos=table" >}}
---
# assets/default/ui/map_selection/ui.yaml
sequences:
  highlight_select:
    next: "repeat"
    input_reactions: # Similar to character selection
    frames: [
      { wait: 1, sprite: { sheet: 2, index: 0 }, tint: &colour_select { r: 1.0, g: 0.4, b: 0.1 } },
      { wait: 1, sprite: { sheet: 2, index: 1 }, tint: *colour_select },
      # .. Frames for sprite index 2 to 22 are similar.
      { wait: 1, sprite: { sheet: 2, index: 23 }, tint: *colour_select },
    ]

  highlight_confirmed:
    next: "repeat"
    input_reactions: # ..
    frames: [
      { wait: 1, sprite: { sheet: 2, index: 0 }, tint: &colour_confirmed { r: 0.15, g: 1.0, b: 0.3 } },
      { wait: 1, sprite: { sheet: 2, index: 1 }, tint: *colour_confirmed },
      # ..
      { wait: 1, sprite: { sheet: 2, index: 23 }, tint: *colour_confirmed },
    ]
{{< /highlight >}}

## What's Next

Now that the user interface is *usable*, the next development period will focus on adding content, and that ever elusive game ending score board.

Am approaching a crossroad for whether to continue on this project, or to re-enter the working world, so the next cycle will be crucial to finalizing that decision.

Peace ✌️

## Support Me

The game is still in development; if you would like to support me, please consider becoming a [Patron](https://www.patreon.com/azriel91).

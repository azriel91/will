---
title     : "That Looks Good On UI"
date      : 2019-11-08T06:50:26+13:00
categories: ["post"]
tags      : ["development", "release"]
---

> 📋 "I like the menu."
>
> "Because it lets you choose?"
>
> 🥺 "No, because it's pretty."

This update lays the foundation for better user interfaces (UIs) and menus.

{{< youtube 3-YTFYoa-Qs >}}

What's new:

* Animated menus and backgrounds can be defined.
* Player names can be specified in controller configuration.
* Winner is displayed when a game ends.

Behind the scenes, the following code maintenance has been made:

* Moved all tests into separate crate -- 1.9x speedup, 65% less disk usage ([post]({{< ref "/post/2019-10-08_dev_time_optimization_part_1.md" >}})).
* Assets are loaded into separate asset components -- easier to share logic between different types of objects.
* Asset loading is done in stages, in preparation for the ability to disable certain stages.

The new version can be downloaded on [itch.io](https://azriel91.itch.io/will).

## Play

### Win Status

{{< figure src="/media/2019-11-08_that_looks_good_on_ui/winner_status.png" caption="The winning player is indicated when the game ends." >}}

When the game ends, the winner will be displayed.

## Create

### Configuration

Each game state may be configured with background images. These go under the `assets` directory as follows:

```bash
assets/default/ui
│
├── game_mode_selection  # ID of the state, more information in the blog post.
│  │
│  ├── background.yaml
│  ├── sprites.yaml
│  ├── ui.yaml
│  │
│  ├── clouds.png
│  ├── glow.png
│  ├── heat_artwork.png
│  └── will_800x600.png
│
└── ..
```

Each game state's background images are automatically loaded from directories that are named with the game state's identifier. These are currently:

* `character_selection`
* `game_mode_selection`
* `game_loading`
* `game_play`
* `loading`[^1]
* `map_selection`

The purpose of each `.yaml` file is as follows:

* `sprites.yaml`: Declares the sprite sheets for the state's UI. This is the same as character and map configuration.
* `background.yaml`: Specifies the positions and animation sequences for background images.
* `ui.yaml`: Defines the menu items to display in the state.

In `background.yaml`, animations are defined with a familiar format:

{{< highlight yaml "linenos=table" >}}
layers:
  logo:
    position: { x: 267, y: 400, z: 10 }
    frames:
      - { wait: 10, sprite: { sheet: 0, index: 0 } }

  heat:
    position: { x: 0, y: 0, z: 9 }
    frames:
      - { wait: 10, sprite: { sheet: 2, index: 0 } }

  clouds:
    position: { x: 0, y: 0, z: 0 }
    frames:
      - { wait: 10, sprite: { sheet: 3, index: 0 } }
{{< /highlight >}}

UI behaviour is still a work in progress, but `ui.yaml` will be used to define the UI widgets to allow players to select the game mode, characters, and map to use.

## What's Next

Game play content hasn't been updated this time, but hopefully there will be time for that in the next update. Efforts will first be directed towards improving the user interface, and content improvements will be made with what remains. Specifically, the next period will focus on:

* Improvements to the character and map selection menus.
* Displaying control buttons.
* Better winning scoreboard.
* Content improvements with remaining time.

Forward!

## Support Me

The game is still in development; if you would like to support me, please consider becoming a [Patron](https://www.patreon.com/azriel91).

[^1]: Configuration and assets are loaded during the `loading` state, so they may not show up during loading.

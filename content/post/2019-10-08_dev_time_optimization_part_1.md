---
title     : "Dev Time Optimization -- Part 1 (1.9x speedup, 65% less disk usage)"
date      : 2019-10-08T13:10:38+13:00
categories: ["post"]
tags      : ["development"]
---

## Summary

In a 45k LOC / 102-crate workspace, moving tests from member crates into a single `workspace_tests` crate achieved the following improvements:

* Build and test duration in release mode reduced from 23 minutes to 13 minutes .
* Debug artifact disk usage reduced from `20 G` to `7 G` (65% reduction, fresh build), or `230 G` to `50 G` (78% reduction, ongoing development)

## Background

The rate of software development is affected by many limits. Two of these are:

* **Hardware:** Disk space, memory, CPU.
* **Patience:** How much one is willing to tolerate -- wait times, software chores, spurious errors.

In building Will, these limits had been (b)reached. Money can't buy patience, and there isn't enough for more hardware 😛. So I did what every developer does when it takes too long to write software..

Write more software!

## Method

> **Note:** This method / advice is from [Robert O'Callahan](https://robert.ocallahan.org/), credits to him.

1. Move module tests from each crate into a separate `workspace_tests` crate.

    Tests from `a_crate/src/a_module.rs` are moved to `workspace_tests/src/a_crate/a_module.rs`.

    This does mean every type that normally isn't `pub` has to be made `pub`.

2. `cargo test --all`.

## Results

Every 5~10 commits, I pushed to CI to get an incremental view of progress.

```bash
# Commit history
|\  
| * e9e41425 Updated `CHANGELOG.md`.
| * 4f34d9cd Removed `dev-dependencies` from `ui_audio_model`.
| * 018ef48f Moved `ui_audio_loading` tests to `workspace_tests`.
| * c5d9feb4 Moved `tracker` tests to `workspace_tests`.
| * 881f34ee Moved `test_object_model` tests to `workspace_tests`.
| * 44464178 Moved `team_model` tests to `workspace_tests`.
| * 11a9f20d Moved `stdio_input` tests to `workspace_tests`.
| * fde2e158 Moved `stdio_command_stdio` tests to `workspace_tests`.
| * 814e5a9c Moved `sprite_model` tests to `workspace_tests`.
# .. elided
```

{{< figure src="/media/2019-10-08_dev_time_optimization_part_1/cargo_test_wall_time.png" caption="Graph of cargo test duration during the migration." >}}

**Note:** Build number 10 is when Rust was upgraded from 1.36 to 1.37, which enabled pipelined crate compilation. Not sure why Windows dipped at build #8.

### Compile and Test Durations

* **Linux 1 (2012 laptop):**

    | Duration | Before    | After     | Time Saved | % Saved | Speedup |
    | -------- | --------: | --------: | ---------: | ------: | ------: |
    | Compile  | `23m 47s` | `12m 31s` |  `11m 16s` | `47.4%` |   `1.9` |
    | Test     |  `5m 24s` |  `3m 29s` |   `1m 55s` | `35.5%` |   `1.6` |

* **Windows 1 (2016 laptop):**

    | Duration | Before    | After     | Time Saved | % Saved | Speedup |
    | -------- | --------: | --------: | ---------: | ------: | ------: |
    | Compile  | `20m 18s` | `10m 38s` |   `9m 40s` | `47.6%` |   `1.9` |
    | Test[^1] |  `3m 15s` |  `3m  8s` |   `0m  7s` |  `3.6%` |   `1.0` |

* **Linux 2 (2019 laptop):**

    | Duration | Before   | After    | Time Saved | % Saved | Speedup |
    | -------- | -------: | -------: | ---------: | ------: | ------: |
    | Compile  | `3m 53s` | `2m 50s` |   `1m  3s` | `27.0%` |   `1.4` |
    | Test     | `2m 27s` | `2m 30s` |  `-0m  3s` | `-2.0%` |   `1.0` |

<details>
<summary>**► Hardware** (click for details)</summary>

* **Linux 1 (2012 laptop):**

    - CPU: i7-3610QM @ 2.30 GHz (4 cores / 8 processors) -- 4 threads[^2]
    - Memory: 8 GB

* **Windows 1 (2016 laptop):**

    - CPU: i7-6700 HQ (4 cores / 8 processors)
    - Memory: 16 GB DDR4 2133 MHz

* **Linux 2 (2019 laptop):**

    - CPU: i7-9750H @ 2.60GHz (6 cores / 12 processors)
    - Memory: 64 GB DDR4 2666 MHz

</details>

### Disk Usage

Disk usage varies on CI vs local development -- a development directory tends to keep previous artifacts lying around. The following table shows the improvement to disk usage before / after migration:

| Disk Usage          | Before  | After  | Disk Saved |
| ------------------- | ------: | -----: | ---------: |
| Clean Build         |  `20 G` |  `7 G` |     `13 G` |
| Ongoing Development | `230 G` | `50 G` |    `180 G` |

Before:

```rust
// https://github.com/nushell/nushell
nushell> ls target/debug/ | where size > 100mb | pick name size | sort-by size | reverse
━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━
 #  │ name                                                    │ size
────┼─────────────────────────────────────────────────────────┼──────────
  0 │ target/debug/will                                       │ 388.5 MB
  1 │ target/debug/game_play-ff48109c12a65823                 │ 330.1 MB
  2 │ target/debug/character_play-71862b5ed391b4a9            │ 298.0 MB
  3 │ target/debug/sequence_play-d16d559316a09064             │ 296.7 MB
  4 │ target/debug/game_input_stdio-03fdda36d5a48d2d          │ 295.9 MB
 // .. elided
 62 │ target/debug/sprite_model-dca0d48bfeba7474              │ 107.5 MB
 63 │ target/debug/map_model-20be732995dc914e                 │ 106.2 MB
━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━
```

After[^3]:

```rust
nushell> ls target/debug/ | where size > 100mb | pick name size | sort-by size
━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━
 # │ name                                            │ size
───┼─────────────────────────────────────────────────┼──────────
 0 │ target/debug/assets_built_in-c6e394b4c4bc2bf2   │ 109.4 MB
 1 │ target/debug/character_loading-fad1bc4c36f3147e │ 119.1 MB
 2 │ target/debug/will                               │ 389.1 MB
 3 │ target/debug/workspace_tests-5abfb3c4396f2427   │ 413.4 MB
━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━
```

## Takeaways

* Manually moving tests from 100 crates is somewhat tedious -- to maintain crate structure, creating files / directories by hand takes time. It have been may be worth scripting.
* "Put all your tests in one crate" is still a trade off decision -- need to expose non-API types and functions.
* Strict code organization helps a lot.

## Future Work

There wasn't much speed up for the test execution on the faster machines, as many tests are forced to run serially[^4]. Many of these can be parallelized by not loading graphics in those tests (hint: easier with the ECS paradigm).

[^1]: Not measured using a `time` command, but subtracted the compilation time from the total build duration.
[^2]: Only 4 processors used due to insufficient memory during linking -- `cargo test --all -j 4`.
[^3]: `assets_built_in` and `character_loading` don't contain any tests, but they do use `lazy_static`, perhaps that's causing the binary to be created, but I haven't confirmed.
[^4]: On both Windows and Linux, the test binary segfaults when multiple threads interact with Vulkan through `rendy` ([haven't figured out why](https://github.com/amethyst/rendy/issues/151))

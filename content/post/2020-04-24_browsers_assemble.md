---
title     : "Browsers! Assemble"
date      : 2020-04-24T15:44:22+12:00
categories: ["post"]
tags      : ["development", "release"]
---

> ⏳ Time for a break.. ..unless?

## Overview

You can play online, while playing online[^1] 🎮.

{{< youtube Hc8EtqrlJsE >}}

The desktop version can be downloaded on [itch.io](https://azriel91.itch.io/will).

The web version is not yet published as it only runs on Chrome on Linux; on Windows it crashes while loading.

## Open Source

Most of the last 6 weeks was spent on [Web Assembly support](https://community.amethyst.rs/t/wasm-effort/1336/11) in the [Amethyst game engine](https://amethyst.rs/) -- allowing the game to run in a browser. It is far from complete, but the current state demonstrates the potential of using Rust for native and web applications. Much thanks to [`@chemicstry`](https://github.com/chemicstry) for pushing through one of the difficult problems early on.

In the next two weeks, I will be writing a report on the effort towards supporting WASM as an application backend, and likely giving a (virtual) talk at the local Rust meetup.

---

> ⭐ **Achievement Unlocked:** First Contributor

On this side, Will had its [first contribution](https://github.com/azriel91/autexousious/pull/216)! [`@Lighty0410`](https://github.com/Lighty0410) tidied up the code to comply with coding best practices.

For contributions, I spent some time on the contribution process, but haven't yet spent time on *making it easy to contribute*. In detail, what exists:

* Issue tracker with high-level tasks.
* Automated security auditing, license auditing, and code quality checks.
* Automated building, testing, and publishing.

What is not present:

* Issues suitable for first time contributors.
* Contribution guidelines.
* Instant messaging platform for quick questions.

## What's Next

When writing the [previous post]({{< ref "/post/2020-03-13_join_me.md" >}}#what-s-next) I wasn't sure how long more to press on. I've decided after this release it's time to work among people again[^2], and build up some income.

While I haven't yet pledged my time away, I'll chip away at making contributing easier. At that point I'll try and fit something in every now and again.

If you would like this project to continue, please consider:

* [Writing code](https://github.com/azriel91/autexousious)
* [Creating content](https://github.com/azriel91/will_assets)
* Becoming a [Patron](https://www.patreon.com/azriel91)
* Becoming a [Github sponsor](https://github.com/sponsors/azriel91)

Peace ✌️

[^1]: Will *can* run in the browser, but is not yet hosted as it is still unstable.
[^2]: Though it may be difficult given the current pandemic.

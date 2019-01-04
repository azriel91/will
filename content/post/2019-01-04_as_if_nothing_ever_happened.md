---
title     : "As If Nothing Ever Happened"
date      : 2019-01-04T17:18:56+13:00
categories: ["post"]
tags      : ["development"]
---

> Sometimes you build things, other times you break them. Often you must do both.

When building a new feature, oftentimes you try to fit it in with the existing pieces. However, sometimes the pieces don't connect together. That's when the code has to be broken apart, rearranged, and put back together in a way that accomodates everything that was, and that which will be.

In this update nothing has visibly changed, so there are no previews. Behind the scenes however, these bits have shifted:

* Commonized code that updates object animations.
* Split game object components into smaller pieces.
* Use Rust 2018 idioms.

These all correspond to the "Reorganize runtime data and logic" item in the last post.

## What's Next

Next time, the asset loading rewrite should be done, and potentially the automatic collision tests.

It's a short update, but when you can't see the change, it's as if nothing ever happened.

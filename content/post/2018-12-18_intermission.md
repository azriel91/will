---
title     : "Intermission"
date      : 2018-12-18T11:31:13+13:00
categories: ["post"]
tags      : ["rust"]
---

*Response to the [call for Rust 2019 Roadmap blog posts](https://blog.rust-lang.org/2018/12/06/call-for-rust-2019-roadmap-blogposts.html)*

Things I would like prioritized in the 2019 roadmap.

* Reduced Compilation Times
* Refined Tools

## Reduced Compilation Times

This is echoed in many existing 2019 blog posts, so instead of detailing the pain, here is a list of the relief / joy this would bring:

* **Development Experience:**

    Quicker "red-green-refactor" development cycle.

* **Quality:**

    Less impedence in running a large suite of tests results in smaller, more comprehensible changes.

## Refined Tools

* **Tidy:**

    Rust artifacts are ever growing. My disk space does not. It would be nice to have automatic clean up of unused files:

    - Stale compilation artifacts.
    - Old versions of crates in `~/.cargo/{registry, git}`.

    Perhaps deleting based on last access time, and different clean-up profiles for development, CI would solve this.

* **Brave:**

    There are many times Rust provides hints that could be automatically applied without needing human decision.

    - **Automatically managed imports:**

        Java does this well, though I know Rust has more variants to manage &ndash; crate groupings, style.

        Sometimes the suggestions are incomplete, though it's not clear why &ndash; types from the same module export are not consistently listed / unlisted.

    - **Lints:**

        Extra `&`, unnecessary `clone()`? By all means, remove them.

    Cargo fix is already quite brave, and deserves a million applauds. Allowing more tools to change code without explicit prodding could organically induce coding best practices.

* **Refactoring:**

    There are times I'd like to change a method signature, add or split a parameter, or move a module. For a single-developer project that follows strict conventions, regex is possible, but is nonetheless fragile. There are some moments where I'd miss mature IDE support that is available with languages such as Java.

    Quite confident that the [RLS][rls] will get there, the question is how soon, and whether we remember "let's support this in cargo workspaces too!".

* **Less Hungry:**

    Normally I like things that are intense. High CPU, memory, and I/O usage are not among those things. Rust compilation and [RLS][rls] tend to be hungry enough that, for a cold start, my (Windows) machine becomes unresponsive to do any text editing, and my machine isn't all that weak. Linux experience tends to be better.

    There are additional tools that support software maintenance such as [`cargo-outdated`] and [`cargo-tarpaulin`]. However, for a small-medium repository (~28k LOC), these run out of memory (16 GB) and crash, and it isn't possible to realize their value.

    Diverting some attention to the efficiency of these existing tools would ease the work in the quality aspect of the Rust ecosystem.

Wishlist is not explicitly game-dev focused, but game-dev will certainly benefit from all of these enhancements.

[`cargo-outdated`]: https://github.com/kbknapp/cargo-outdated
[`cargo-tarpaulin`]: https://crates.io/crates/cargo-tarpaulin
[rls]: https://github.com/rust-lang/rls

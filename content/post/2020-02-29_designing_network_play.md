---
title     : "Designing Network Play"
date      : 2020-02-29T23:33:44+13:00
categories: ["post"]
tags      : ["development"]
---

> 📡 A simple session server is an online hash map. 🗺️

This post goes through the design process for implementing network play in *Will*.

It's my first time implementing a networked game, so design improvements certainly can be made.

## Usage Scenario

From players' perspective, one game client will host a session, and other players join that session. On top of that, each game client may have multiple players. An example scenario to support:

1. **Player A and B:** Host a session.
2. **Player C and D:** Join session.
3. **Player E:** Join session.
4. 🎮 Play.

## Solution Design

### Constraints

To make the process simple for players, we impose the following constraints:

* Players should not have to know about IP addresses.

    - Asking the session host to figure out their external IP address is not a good user experience.
    - Asking session joiner to enter an IP address is not ergonomic.

* Players should not have to forward ports on their modem / router.

    For an application to be the first to *listen* for network communication, the modem (exposed to the internet) must open a port, and then forward that to the computer that the application is on.

    However, if the application *initiates* the communication, then the device it communicates with is free to communicate back, and the application can listen without the port forwarding setup.

    Asking users to open ports is also a security concern, and hence unreasonable.

### Solution Selection

To remove the requirement for opening a port, an external server can be used to listen for incoming connections from game clients, and reply to those connections. This shifts the risk from the players to the server.

To make it easy for players to play together, the server manages *sessions* -- groups of connected clients. The server generates a session code, to which each player joins. Entering a 4 letter code such as `ABCD` is far easier than `111.70.32.58`.

The server manages communication between the clients by sending control input from clients in the session to each other[^1]. There are [other methods of game networking](https://gafferongames.com/post/what_every_programmer_needs_to_know_about_game_networking/), but for this attempt I will use the simpler method.

### "Conversations"

To define how the game client and session server communicate, it is valuable to work out the conversation flows between those *two* participants, even if the conversation is part of a process involving more participants.

For example, the following steps describe the process of hosting and joining a session:

1. The session host requests a session code from the server.
2. Server generates a session code and sends it back to the host.
3. A joining player sends a request to the server to join the session, passing in the session code.
4. The server accepts (or rejects) the join request.
5. The server notifies the session host of the joiner, so the other player is shown on screen.
6. The session host tells the server that game play should begin.
7. The server notifies all clients to begin game play.

If we look at this carefully, there are two separate conversations happening -- one between the session host and the server, the other between the session joiner and the server. Let's look at these individually:

#### Hosting

The conversation between session host and server happen in steps 1, 2, 5, 6, and 7:

1. The session host requests a session code from the server.
2. Server generates a session code and sends it back to the host.
3. <span style="opacity: 0.3">A joining player sends a request to the server to join the session, passing in the session code.</span>
4. <span style="opacity: 0.3">The server accepts (or rejects) the join request.</span>
5. The server notifies the session host of the joiner, so the other player is shown on screen.
6. The session host tells the server that game play should begin.
7. The server notifies all clients to begin game play.

#### Joining

The conversation between session host and server happen in steps 3, 4, and 7:

1. <span style="opacity: 0.3">The session host requests a session code from the server.</span>
2. <span style="opacity: 0.3">Server generates a session code and sends it back to the host.</span>
3. A joining player sends a request to the server to join the session, passing in the session code.
4. The server accepts (or rejects) the join request.
5. <span style="opacity: 0.3">The server notifies the session host of the joiner, so the other player is shown on screen.</span>
6. <span style="opacity: 0.3">The session host tells the server that game play should begin.</span>
7. The server notifies all clients to begin game play.

#### Playing / Synchronizing Input

After game play is established, both the session host and joiners have the same conversation with the server:

1. Game client sends input to the server.
2. Server combines input from all game clients, and sends them back to each client.

> **Implementation note**
>
> It is possible to write code that sends messages that do not follow the conversation script. However, we can ensure
> at compile time that the server and clients respect each conversation flow with a *session type* library such as
> [`session_types`](https://crates.io/crates/session_types)

## More Cases

So far we've looked at the simple case. There are many situations the design does not handle:

* How do we *end* a session cleanly?
* What should happen when the session server crashes?
* What should happen when a game client disconnects?
* What if a player wants to reconnect after disconnecting?
* What if a new player wants to connect mid-game?
* What if game clients have different characters?

For good user experience, these should all be detected, and where viable, players presented with options whether they would like to recover and continue the game.

## Postamble

As always, designing and implementing something that works in the ideal case takes much effort, and yet is only a small fraction of designing something that doesn't break when everything else does.

Now there are some fuzzy blueprints, let's build it.

## Support Me

The game is still in development; if you would like to support me, please consider becoming a [Patron](https://www.patreon.com/azriel91).


[^1]: Alternatively, one could implement [UDP hole punching](https://en.wikipedia.org/wiki/UDP_hole_punching). However, I intend for the game to eventually compile to a WASM application, and may have to fallback to a HTTP request client.

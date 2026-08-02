# The Standalone Game (the 14-year-old's dream — where this is actually going)

> Wildcard Arena the SC2 mod is not the destination. It's the on-ramp. The destination is an **original standalone RTS** built around the idea I've been turning over since I was ~14: *you don't pick your army, you're dealt one, and you win by reacting to your hand.* This doc exists so that vision lives in the repo instead of evaporating between conversations. Nothing here is a commitment or a schedule. It's the map, written down so I can set it down.

## The north star: Battle Aces scale, original identity

Battle Aces is the reference point — **not** for its art, but for the decision underneath it: the game is **small on purpose.** Small army counts, tight unit variety, short matches. That reads as an aesthetic/onboarding choice, but it's secretly the choice that makes a solo-built multiplayer RTS *possible* (see Netcode below). The scale and the dream are the same lever.

Two pillars, stated plainly because they will pull against each other in engine choices:
1. **Visual clarity** — a newcomer should be able to *read* a fight without 500 hours of ladder muscle memory. This is the differentiator and the constraint. It's also *harder* than SC2, because I don't get to lean on expectations a StarCraft ladder hero already carries — I have to teach readability from scratch, in the art and the systems.
2. **Working online multiplayer** — a hard MUST. This is the part that kills most solo RTS projects.

## The core design: modular units (base + weapon slots + gadget slots)

The original vision, in full (the mod cut most of this for scope — here it earns its way back, because it's *more* coherent as an original game than as an SC2 mod, where fixed unit art fights it):

- **Randomized base / chassis.** A unit's base is rolled — e.g. a "Stalker base" comes with that unit's move speed, armor, HP, shields, size. The chassis defines the *body*.
- **Randomized weapon in a slot.** The weapon attached to the chassis is rolled separately from the base.
- **Sized weapon slots.** Each chassis has some number of weapon slots, and slots have **sizes** — a Thor-class chassis has large slots, a Marine-class chassis has mini slots. Big body, big guns; you can *see* the threat. This is readability baked into the geometry.
- **Gadget slots.** Each unit can carry actives (e.g. blink) or passives (e.g. concussive shells) in gadget slots.

Why this serves Pillar 1: it replaces "memorize 40 units and their matchups" with a **build-a-unit grammar you can read off the parts.** The slots and their sizes *are* the tell. That's the whole point.

## Netcode: the scariest part, and why the design already tames it

Two ways to do RTS multiplayer:

1. **Deterministic lockstep** (StarCraft, AoE, the Recoil/Spring engine). Sync only *inputs*; every client re-simulates the game identically. Bandwidth-cheap even with huge armies. The dragon is **determinism** — every client must compute *bit-identical* results forever, or it desyncs (the most miserable bug class in game dev). Serious lockstep RTS use fixed-point math and treat determinism as a religion from line one. It cannot be bolted on later.
2. **Server-authoritative state replication** (shooters, MOBAs). Server owns the truth and streams state; nondeterminism is a non-issue because the server corrects clients. The cost: **bandwidth scales with unit count** — death for a 200-supply macro RTS, but **fine for a small-army game.**

**The punchline: a Battle-Aces-scale game unlocks the easy netcode path.** Keep armies to dozens of units, not hundreds, and server-authoritative replication becomes viable — sidestepping the determinism nightmare entirely. The biggest fear (netcode) and the north star (small scale) are solved by the same decision.

### Rollback: intentionally NOT doing this
Rollback works for fighting games because they have tiny state and demand frame-perfect input — cheap to re-simulate a few frames of two characters. RTS is the opposite: enormous state (re-simulating thousands of units per rolled-back frame is brutal) and **latency-tolerant** — 150ms of command delay is invisible in an RTS and lethal in Melee. Essentially no RTS ships rollback. "Nice-to-have" undersells how safely this can be dropped. Filed under *never*, not *later*.

## Engine: Recoil vs. Godot — a real fork, tied to the two pillars

- **Recoil Engine** (open-source, powers Beyond All Reason / Zero-K). Solves the scariest problems *for free*: netcode, pathfinding, large-scale unit sim, decades of RTS hardening. But you inherit its world — an older, opinionated C++/Lua codebase with a strong baked-in Total-Annihilation look. Bending it toward a novel modular-unit system and an original visual identity is fighting the current.
- **Godot** — total control over the thing I care about most (clarity + a look nobody's seen), modern and pleasant. But I own the hard parts, and RTS starter packs almost always punt on the one that matters (real netcode; hotseat/naive sync doesn't count).

**The tension, named:** the two pillars point at different engines. Recoil buys the netcode but taxes the clarity/identity; Godot buys the identity but charges the netcode. (I've already pulled down a Godot RTS starter pack; I'm currently leaning Recoil for the netcode reason.)

## The way through: validate fun before infrastructure

This is the lesson the mod already taught me — **prove it's fun with my own hands before betting years on the plumbing.**

I do NOT need netcode to answer the only question that matters right now: *is a modular-unit RTS actually fun to play?* Build the core loop **single-player or hotseat in Godot**, where iteration is fast and the look is mine. Prove the base + weapon-slot + gadget system is fun. *Then* pick the netcode/engine battle — with a fun prototype in hand and evidence it's worth the years. Don't let the scariest problem gate the cheapest, most important experiment.

## Posture: this is the beginning of the beginning

- Wildcard Arena Season 1 is nearly a shippable product — and that's the *on-ramp*, not the destination. Its real job beyond being fun: **cheaply validate the "react to your hand" core idea with real players** before I commit to the standalone.
- Take my time. Years, not months. That's correct, not a failure.
- **Lean into AI** — it collapses the cost of the surrounding 90% (glue, tooling, UI, content pipelines, netcode scaffolding). Keep *architecture* decisions human-owned: the determinism-vs-server call, the netcode model, the core sim. AI executes those beautifully once I've decided them; it will happily hand-wave past a desync if I let it drive.

## The next concrete rung (not "start the game")
1. Ship Wildcard Arena Season 1; watch whether people love the *idea* (not just the mod).
2. On the side, spin up a **Godot prototype of the modular loop** — hotseat, no netcode — purely to answer "is this fun in my hands."
3. Everything else (engine commitment, netcode, art identity) waits behind that yes.

## Seeds already in this repo that feed the dream
- The **per-slot randomization + tag system** is a working, playtested implementation of "you're dealt an army" — the concept's core, already proven fun.
- The **modular upgrade grammar** (abilities / behaviors / count-upgrades attached to rolled units) is a first draft of gadget slots.
- The **community, marketing, and shipping muscle** being built now transfers wholesale.
- See [`forge-mechanic.md`](./forge-mechanic.md) — the other big "someday" idea, cut for the same scope reasons.

# Design principles

The core tenets that steer Wildcard Arena's balance and feature decisions. When a change is ambiguous, check it against these. Living doc — add tenets as they crystallize.

---

## North star: react to your hand, not a plan
Standard SC2 rewards a rehearsed plan ("8 gateways, 56 workers, twilight, attack with stalkers"). Wildcard Arena's identity is the opposite: you're dealt a random hand of units + upgrades and you have to **react** — to your roll and to your opponent's. Every decision serves this. If a change lets a player stop reacting and run a rote plan, it's working against the mod, even if the numbers look fine.

---

## The Simple Game Plan Effect
**The single most important balance principle. Read this before touching any unit's cost.**

**Cognitive load is not in the numbers.** A player massing one unit and A-moving spends far less attention on decisions than a player reading their hand and mixing a composition. At *equal skill*, the simple-plan player executes cleaner and makes fewer mistakes. Therefore:

> **"Balanced on paper" = "too strong in practice"** for any low-cognitive-load strategy. To equalize the actual games, the lazy strategy must be *weaker on paper.*

And for this mod it's not merely balance — a viable single-unit-mass plan lets players **opt out of the reactive gameplay that is the entire point.** So:

> **Single-unit-mass should be a trap, not a plan.** Keep mass-able units *slightly underpowered on paper.* That underpowered-ness is the fair price of the cognitive-load discount they already enjoy.

### The lever hierarchy (buffing / nerfing a mass-able unit)
Levers are not equal against this effect:

- **COST — the dangerous lever. Never *reduce* cost on a mass-able unit.** Cost-efficiency directly strengthens mass-as-an-economic-plan; this is what flattens the game. (A cost *increase* to nerf an over-strong mass unit is fine — that's aligned with keeping it underpowered.)
- **BUILD TIME — the safe buff.** Raises output *per production building*, but the investment tax (building cost + no warp-in) stays. Fixes "under-produced even when I commit hard" without making the mass-plan cheap or lazy.
- **The ROLLED UPGRADE — the best lever.** Makes a unit shine in a *specific* situation, so it **rewards reading the game** rather than lazily massing.

**Heuristic: tune for "strong in a mix, weak when massed."** Use build-time and the rolled upgrade (help a unit in a reactive comp); avoid cost *reductions* (help the mass-plan). High building costs and the absence of warp-in are **features** — the friction that punishes lazy massing. Don't "fix" them.

### Two layers of protection
- The **roll system** kills the *pre-game* plan (you can't count on rolling a specific unit).
- **Keeping single-unit-mass weak** kills the *in-game* lazy plan (A-moving the one thing you happened to roll).

Both are needed to force thinking at both layers.

### Evidence (why we believe this)
- **Reduced-cost blink Hydralisks:** mass blink Hydra *won against an opponent who was genuinely drafting a composition to counter it.* The lazy plan beat the reactive one — textbook failure mode of over-cheapening a mass unit.
- **Marine at 50/10:** a friend just showed up with mass Marine + a few Liberators and trashed us → bumped to **50/25** (the "flatten the mode" fix). See the patch note / `WA-016`.
- **Marine at 50/25 — open question.** Even at the higher price, opponents *still* commit serious Marine counts, and we lost most of those games. Marine is *the* canonical low-cognitive-load unit (a-move, stim, stutter-step), so it may overperform even at a "fair" price — exactly what this principle predicts. **Watch:** 50/25 may still be only "fair," not yet "slightly underpowered." If it needs a further tax, use a **cost increase** or a production/build-time nerf — not a buff to its counters.
- **Factory Stalker build time 25 → 18 (PR #16):** deliberately a *build-time* buff (safe lever), **not** a cost cut — to fix Stalker under-production without making mass-Stalker a cheap plan. Shipped as a hypothesis to test against this very effect. Symptom to watch: does "just mass what I rolled" start beating reactive play at equal skill? If so, walk 18 back toward 20–22 — never toward cost.

---

## Corollary: noticeable upgrades over invisible ones
Generic +1/+1 attack/armor upgrades add little strategic depth — you rarely *know* you're losing to an upgrade disadvantage until you watch the replay. Wildcard Arena drops them for **visible, decision-driving** rolled upgrades (Blink, blue flame, range, transform speed…). An upgrade should change **how a unit micros or what it's good against** — something a player can see and react to. This also feeds the north star: a situational upgrade rewards reading the game.

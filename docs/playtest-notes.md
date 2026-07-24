# Playtest notes

A running journal of play sessions — half **balance/design observations**, half **personal-play self-critique** (a "get better at my own game" log for someday, once the game feels like it's in a good spot). Newest session on top. Dictated after games, cleaned up but kept in my own voice.

---

## 2026-07-23 — ~5 games vs an even-skill opponent (won ~2: games 1 & 4)

**The big takeaway:** this session *was* the design working. I scouted mass Marines, thought "straight Zealots + Firebats might actually beat this," and got to play my hand out. You can't bring a build order into this game — you react to what you're dealt. That's the whole point, and it delivered.

### Game — Zealots/Firebats (+ air) vs Marines/Colossus → **WIN**
- Opponent went Marines + Colossus (+ probably something I'm forgetting).
- I went Zealots + Firebats, **Blink on the Zealots**. Started adding air once I saw all the Colossus.
- Scouted the mass Marines early → decided to commit to Zealots + Firebats. It worked.
- All my Firebats died during a long siege. Then I remembered I had **Blink on my Zealots** — blinked a Zealot onto a Colossus and killed it. (Great moment. Forgot I even had it until late.)
- Won the big fight → got the lead → went home, built extra production (~**10 barracks, 5 stargates**, 1 factory I never used), extra bases, teched up, then laid siege.
- Threw my army in to die on purpose to free supply, while mutas killed workers. Felt safe, but he snuck back and killed a base — got a little scary. Remaxed fast (more mutalisks this time), hit multiple bases at once, and kept enough Zealots/Firebats to clear the Ravens sieging my base.
- Also the game where I had **Ghosts with auto-turrets** — just threw turrets at him, super fun.

**Self-critique:** I over-micro'd (I'm bad at muta micro) and *forgot to build units* — a game I was clearly winning dragged out way longer than it should have because of it. Probably overcomplicated it; could've just kept it simple and macro'd.

### Game — factory/vulture opening; Queens/Lurkers/Thors/Vikings → **LOSS** (vs Colossus/Siege Tank/Hydralisk/Liberator)
- Was hell-bent on a factory build. Knew this guy plays safe/greedy every game.
- Opened Supply Depot → Barracks → CC → Factory + a bunch of Vultures. Micro'd my heart out harassing, but didn't get much value — he hard-walled, so the ultras couldn't do much either. Mostly just made him micro a bit early.
- Got a third, went ~7–10 workers ahead. But I got **too greedy** — built way too many Command Centers, over-invested in economy when the game actually called for hunkering down (he wasn't expanding fast). Expanding faster didn't pay off this game.
- Lost the Queens/Lurkers/Thors/Vikings comp to his Colossus/Siege Tank/Hydra/Liberator.

**Self-critique:** Read the *situation*, not just "always expand." When the opponent is passive and not taking bases, the fast-expand doesn't automatically win — sometimes you consolidate and punish, not sprawl.

### Game — Twin-Linked Flamethrower Hellions → **LOSS** (probably Ultralisks + more)
- Committed to Twin-Linked Flamethrower Hellions. It was **terrible**. Big caveat: I threw my entire Hellion count away *right before* the upgrade finished, and I was already so far behind by then that there wasn't much else to do — so not a fair read of the upgrade.
- **The third-base interaction (interesting):** I was the more-aggressive/faster-expanding player again. Just threw all my Hellions, started rebuilding, and sent the new batch to his side the second Twin-Linked finished — and *then* 3–4 Hydralisks showed up at my undefended third. All I had was a Queen and a Goliath on the way. The Queen nearly died before the Goliath arrived; I *won* the fight (his Hydras left at ~1 HP each, my returning Hellions cleaned up) — but he'd already sniped the SCV building my CC. **Won the fight, lost the momentum** because my army was scattered all over the map.
- He walled his third (CC + 2 bunkers + supply depot + CC) on a ramp map — my Hellions couldn't get in. A gap opened later but he had enough units by then, and I didn't micro them well.
- Tried to transition into **Void Rays with Concussive Shells** — the concussive beam was **totally useless**. Heavy caveat: I was so far behind that Void Rays with *any* upgrade would've been bad, and I was using them for chip harass with no muta support — not a real test. Maybe good off a proxy; not a clean data point.

**Self-critique:** Threw the Hellions away right before their upgrade landed (terrible timing). Over-aggressive/over-expanding again. Scattered units cost me even a fight I won.

### Game — Blink Stalker w/ Blink-Cooldown-Reduction → **LOSS** (behind, self-inflicted)
- Went Blink Stalker with the cooldown-reduction upgrade. Didn't feel OP. Bad opener (didn't commit to it until late), and he walked off the natural's low ground so harass was hard even with Blink.
- Whole game I kept thinking "I'd really like to blink *now*" — the **3.5s recharge felt longer than I expected** for the upgraded cooldown. Can't tell if Blink Stalkers just aren't good or I slowed myself down with the bad opener. Not a clean read.

### Balance / design observations
- **Firebat + Leap** — felt **strong** two games running now (called it out both times, unprompted). Likely genuinely good — worth a real look / possible tuning target.
- **Blink cooldown reduction (`stalkerblinkcooldown`)** — did *not* feel OP; the **3.5s recharge felt longer than expected** (kept wanting to blink and couldn't). Confounded (behind, uncommitted opener, he walled the natural). Open question: is the CD upgrade under-tuned, or were Blink Stalkers just bad *this* game?
- **Auto-turret range 5** (WA-062) — did **not** feel oppressive. The "can't place them up a ramp without vision" limiter keeps it honest. Feels like the change landed right. (Could imagine someone spamming them from afar, but the vision requirement caps it.)
- **Blink on Zealots** — excellent; blinking onto a Colossus to kill it was a highlight.
- **Queen feels too slow — recurring concern (came up twice).** Legitimately unsure if the Queen is *good*. She's slow enough that a Hydralisk player just out-micros her, and there isn't much micro to do with her. Maybe I'm over-relying on Select-All-Army; maybe I'm under-building them (few transfuses landed). **Worth revisiting her speed / role.**
  - **Direction (2026-07-24, tentative — needs more testing):** leaning toward *keeping* her slow and adjusting **price** instead of buffing speed. Open question to test: is the real pain **cost-efficiency** (price fixes that) or **tempo** (she arrives late to fights — which price does *not* fix; that was the actual third-base problem)? Watch which one it is before committing.
- **Twin-Linked Flamethrower Hellions** — felt terrible, but confounded (threw them pre-upgrade + already way behind). Needs a clean test before judging.
- **Concussive Shells on Void Ray** — felt useless, but confounded (far behind, harass-only, no muta support). Not a clean read; retest when even.

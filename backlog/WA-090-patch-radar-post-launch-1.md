---
id: WA-090
status: in-progress
size: M
phase: 2-post-launch
priority: 50
---
# Patch radar — first post-launch balance pass (late Aug 2026)

Single-glance state of everything on Taylor's radar for the upcoming patch. Living doc; move items to their own tickets when they become actionable. Guiding lens: [[balance-for-forced-creativity]] (below) — don't over-tune into an a-click monster; force creativity for BOTH players, not one.

## ✅ DONE this session (already committed to main)
- **Tempest cost 250/175 → 275/200** + **build time +4s on Faster** (58.8→64.4 catalog). *(Taylor said "I'm gonna do that" — it's already shipped.)*
- **Tempest keeps Stim** — playtest showed stim is self-limiting on Protoss (20%-Life floor blocks casting below 20% HP; Protoss can't be repaired/healed → ~4 casts max). It's an upgrade *choice* (4 stims vs +2.5 range), not a free power-up. (WA-088 exclusion reverted.)
- **Marine lifesteal 15% → 40%** — Taylor happy with it.
- **Diamondback base range 6 → 7** — **REVERTED 2026-08-29** (back to base 6). Taylor wants more data before buffing. Moved to WATCHING.
- **Seeker Missile hotkey R → G** (stale GameHotkeys override; was uncastable on G).
- **Yamato (YamatoU) pinned to 240** (ladder value) in own data.

## 🟡 LEANING (likely this patch, no code yet)
- **Revert Raven mineral cost 75 → 100.** The buff had no strong rationale and mass-Raven felt oppressive + a-click, not micro-rewarding. Low-risk removal of an unjustified discount. See [[WA-083]] (2026-08-27 update). **Separable from Seeker Missile damage** (that stays a watch).
- **Stalker build time +2–3s.** Stalkers feel good but not oppressive; a small build-time bump is the cautious nudge. Uncertain — hold unless it keeps coming up.

## 👀 WATCHING (do NOT act yet — confounded / need cleaner data)
- **Seeker Missile damage.** Taylor lost to it partly because he didn't know the dodge (run the marked unit → fizzles). No game yet where the defender understood it → "too much damage?" unanswerable. See [[WA-083]].
- **Ghost Academy gas price / build time (tech-rush pacing).** Recurring eyeball (months). Concern: first upgrades getting rushed out and *deciding the game before it gets off the ground* — but Taylor also doesn't want every game to be a long macro game, nor an "a-click as soon as the upgrade finishes" race. Not pulling the trigger. If acted on, lean **slight build-time** over gas price. Gather even-game data first.
- **Blink Zealot one-base all-in.** See [[WA-087]] — confounded by greedy openings.

## 🛑 HOLDING THE LINE (decided — don't relitigate)
- **No generic +1 weapon/armor upgrades.** Common feedback ("sad I can't +damage"), but declined. Reasons: (1) **balance multiplier** — a generic +N scales *every* per-unit upgrade combo (e.g. +3 stim Tempests), turning one lever into a coefficient on everything; (2) **UI** — no room / no more buttons on the Ghost Academy card; (3) **core differentiator** — "every upgrade changes how a unit plays, not its numbers" IS the pitch. Mitigation instead: make the absence *intentional and visible* in copy (mod description / site "Why It's Different") so the feedback shifts from "missing/broken" to "that's the point." (Website copy is the separate site repo/agent.)
- **No upgrade-pool changes / no structure remix** (warp gate, larvae, non-Terran hotkeys) — keeps the mod balance-able and avoids the Terran-hotkey learning wall that already turns some players off.

## Balance methodology reminders (Taylor's own, this session)
- His "OP" read **skews by opponent skill** and by **whether he understood the mechanic** — most "felt OP" games this session were actually "I was behind first" or "I didn't know the counter." Separate *imbalance* from *I-was-behind/confused*; balance on **even-game data**, not feels-while-losing.
- **Convergent, unprompted, from-players-who-played** feedback (e.g. the removed-upgrades complaint from 3 separate commenters) is the strongest signal — much more reliable than a single replay.
- Goal is **forced creativity for both players**, not an a-click monster. Over-balancing can flatten the game back into a-move. See [[balance-for-forced-creativity]].

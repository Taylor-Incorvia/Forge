# Community & events model

How Wildcard Arena builds and sustains a player community and event scene. The companion to [design-principles.md](./design-principles.md): that doc steers the *game*, this one steers the *community around it*. Living doc — add lessons as they crystallize.

The north star for the community: **build the scene so it survives you having a life.** A volunteer-run event scene is *labor*, and that labor is what burns out organizers and ends leagues. Every decision below is filtered through "does this depend on me hand-cranking it, or can it run without me?"

---

## 1. What we're copying from StarCraft Evolution League (and the warning)

**SEL** (biweekly tournament for the *SC: Evo Complete* BW-vs-SC2 mod) is the template. Over ~2 years it built — around a **mod**, not the base game — recurring tournaments, a Discord community, Liquipedia pages, Patreon + Matcherino funding, volunteer casters, regular players, and ~$17k cumulative prize pools. That proves the whole thesis: **people will organize around a custom StarCraft experience.** The Discord matchmaking-bot idea came directly from watching their Discord work.

**Adopt the structure:**
- A **regular cadence** (a recurring event people can count on).
- **Radically low barrier to entry** (SEL had no MMR / no region locks — inclusivity is why a niche mod sustained a scene).
- **Discord-first** coordination hub.
- **Community funding** (Matcherino crowdfunded pots + Patreon) so prize pools never come out of our own pocket.
- **Liquipedia / Challonge / TeamLiquid** for outside credibility.

**The warning (the load-bearing lesson):** SEL ran ~22 biweekly editions plus multiple seasons — and then ended when a volunteer organizer stepped away (frustrated with Blizzard/live-game direction). Whatever the exact stated reason, **the default failure mode of every community-run tournament is organizer burnout.** SEL had a *team*; even so, it ended. So:
- **Don't be the single point of failure.** Recruit co-organizers early. If the scene depends entirely on me, it ends the day I'm tired.
- **The Discord bot is burnout insurance, not just a feature.** It's what lets the ritual run without me hand-managing every bracket. (See §4.)
- **This is a phase-2 concern.** A tournament scene needs enough players *first*. Don't stand up a biweekly league before there are players to fill it — running events for 8 people is how you burn out. The funnel (§2) is the on-ramp; the scene is the destination.

---

## 2. The first-week funnel — the real product right now

The single most important question, and the whole game until it's solved:

> **Can someone get 5–10 fun games in their first week?**

Not ladder population, not Blizzard's balance council, not ESL/GSL — those matter much later. The funnel:

```
Reddit post → download mod → understand what's happening → find an opponent → play → have fun → come back tomorrow
```

Every Launch-scope item attacks one stage of this:
- **understand what's happening** → the "Your Faction" modal (WA-001/039) + scout icons (WA-070)
- **not feel lost** → auto-mine at 0:00 (WA-060), hotkey cleanup, docs
- **find an opponent** → the Discord matchmaking bot
- **come back** → the event rhythm (§4)

This alignment is the check: the Launch scope was chosen by instinct and it lands exactly where the funnel analysis says it should. Treat the funnel as *the whole ballgame*, not the mundane part.

---

## 3. The event ladder — reliability before visibility

The brand to build is **reliability, not scale.** SEL earned respect not from big prize pools but because "if there's an event this weekend, it'll actually happen." That brand is built *before* you're visible, not after. The failure mode is inverted order: get eyes on the event, *then* discover it isn't reliable yet — a reputation you can't un-ring.

So run the first events in this order:

**Event 0 — private.** One bracket with friends + Discord regulars. No sidebar, no TeamLiquid page. Purpose: shake out the bot, the rules doc, the mod-version pinning, and the "does it start on time" muscle. If 12 people show up and every one says "that was more fun than I expected," that's a massive success.

**Event 1 — public.** *Then* go for visibility. Checklist before promoting:
- Start date/time (with timezone)
- Prize pool (even $50 — Matcherino/Patreon funded, not out of pocket)
- Registration link (Challonge / Battlefy / Matcherino)
- Discord invite
- Rules + pinned mod version + map
- A nice banner
- A **TeamLiquid event page** (the r/starcraft sidebar event list has historically been curated by mods / pulled from TL's calendar — so a TL page is the on-ramp).

**Getting on the r/starcraft sidebar:** you almost certainly don't have to "qualify" — it's mod-curated / TL-fed. Make the event look *real* (checklist above), then message the r/starcraft mods a week or two ahead: *"I'm running the first Wildcard Arena community tournament — could it be included in the community event list? Here's the TL page and details."*

**Cross-post (don't rely on Reddit alone):** SC2 Custom Games Discords, TeamLiquid forums, and the community Discords of casters (PiG, Wardi, Winter, GiantGrantGames — where appropriate).

**Do NOT** launch with a huge tournament. Small + reliable + fun beats big + shaky.

---

## 4. The weekly rhythm — the asset that's also the burnout risk

The long-term vision is a **rhythm**, not one-off events:

```
Friday   — Discord bot opens the queue
Saturday — Arena Night (casual games)
Sunday   — Weekly tournament
Monday   — Updated Elo / standings
```

The goal state: people stop asking *"Is there a Wildcard tournament?"* and start asking *"What time is this week's Wildcard?"* That's a community, not an event.

**But this rhythm is either the greatest asset or the fastest burnout — decided entirely by whether it runs without me hand-cranking it.** So the rule:

> **Every recurring event must run on automation + co-organizers from day one.** The Discord bot (queue, pair, report, standings, Elo) is what converts "a weekly event Taylor exhausts himself running" into "a weekly event that happens." Build the ritual to depend on the bot and a team, never on my energy alone.

This is the SEL lesson made concrete — and it's also what keeps the project compatible with the rest of my life (see the presence/burnout notes in personal reflections). A ritual that runs itself can sustain a community *and* leave me present for the people who matter.

---

## 5. The ecosystem boundary — inside SC2's ecosystem, outside its meta

Live SC2 does two very different things for Wildcard; hold **opposite** attitudes toward them:

- **The ecosystem — depend on it, care about it.** The client, Battle.net, the Arcade shelf, the audience that returns after a patch, casters browsing for content. This is the storefront and the foot traffic. Wildcard lives here.
- **The balance meta — ignore it.** Council changes, ladder tier lists, what's OP this month. The whole design thesis (per-unit balance, react-to-your-hand) is deliberately decoupled from it.

The crisp statement: **"Wildcard lives *inside* SC2's ecosystem but *outside* its meta."** That's also a marketing line — *you don't have to relearn the meta; every game hands you a new one.*

Corollary: **the pitch is a small ask.** Not "abandon SC2 for a new RTS" (people who love RTS mostly want to keep playing the ones they have) but *"keep playing SC2 — except every match your faction is different."* SEL's ending (a mod scene surviving despite one organizer's frustration with Blizzard) shows a mod's fate isn't chained to the live game's meta. Wildcard's value prop is orthogonal to Blizzard's decisions.

---

## Related
- [design-principles.md](./design-principles.md) — steers the game itself; §2's funnel is why "make it legible" (the Faction modal) is Launch-critical.
- Board: Phase 5 (Matchmaking / Discord bot MVP / Queue Night), Phase 7 (Tournament) — the build tickets that implement this model.

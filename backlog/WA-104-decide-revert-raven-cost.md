---
id: WA-104
status: done
size: S
phase: 2-post-launch
priority: 0
---
# Decide: revert the Raven mineral cost change (75 → 100)?

## The question
Should the Raven's minerals go back to the stock 100 (it was lowered to 75)?

## Taylor's lean: revert
- It's a **change from default SC2 with no strong justification.** The intent was to encourage micro-intensive units, but it's unclear that cheaper Ravens actually delivered that.
- **goog mass-Ravened** and it worked 1 of the 2 times he tried it — enough to be worrying.
- The real uncertainty: is the 75→100 cost the difference between
  - mass Raven being a **slightly annoying but viable** strategy, vs
  - mass Raven being a **really annoying / oppressive** strategy?
  He doesn't know — but reverting an unjustified discount is low-risk.

## Relationship to Seeker Missile
Keep this **separate from the Seeker Missile damage question** ([[WA-083]]). The Raven *cost* is a clean, standalone revert; Seeker Missile damage is still a confounded watch (Taylor hasn't fought it knowing the dodge). Reverting the cost does not prejudge the missile.

## If reverting
Set Raven minerals 75 → 100 in the mod's UnitData (or wherever the mod overrides it). One-line-ish change. Ties off the last live item from the old WA-090 patch radar.

## Notes
Spun out of WA-090 (closed). See also [[balance-for-forced-creativity]] (don't over-tune; but removing an unjustified departure from default is the safe direction here).

## Priority note (2026-09-06)
PRIORITY 0 -- do now + PR: revert Raven minerals 75 -> 100.

## Done 2026-09-06 (PR pending)
Reverted Raven minerals 75 -> 100 in UnitData.xml (back to stock). The buff (100->75, intended to reward micro caster play) never served its goal -- mass Raven + Seeker Missile played as a-click, not APM-rewarding, and was oppressive in volume. Removing an unjustified discount. Seeker Missile *damage* stays a separate watch ([[WA-083]]).

# Decisions (ADR log)

## ADR-001 — Enemy is a donkey, not a beaver
**Status:** accepted · 2026-11-11
The AI enemy is a donkey (خر). All docs, identifiers and events use `Donkey` (`DonkeyEnemy`, `DonkeyStateChanged`, `DonkeyWin`). The Rage warning cue is a bray (عرعر) instead of a growl. The attack can be presented as a kick; rules are unchanged.
Open question: the MVP map is still "Swamp"; a farm/stable map may fit the donkey better.

## ADR-002 — Host mode before paid dedicated servers
**Status:** proposed
Gameplay code stays server-authoritative, but early playtests run in NGO Host mode so fun can be validated before paying for per-match dedicated servers. The same server code later runs headless on Linux.

## ADR-003 — Backend services behind adapters
**Status:** proposed
Authentication, persistence and hosting sit behind interfaces, not only hosting. Reason: Unity Gaming Services and Google Play access may be restricted for the team's region, and the publishing store (Cafe Bazaar / Myket / Google Play) is not decided yet.

## ADR-004 — Stun griefing guard
**Status:** proposed
Because each player gets a personal result, stunning a teammate in front of the donkey may become dominant. Candidates: rescue rewards, reduced Stun on a target the donkey is chasing, per-target Stun immunity window. Decide after playtest.

## ADR-005 — Players are penguins; donkey has three attacks
**Status:** accepted · 2026-11-11
Players are penguins (پنگوئن). The donkey attacks with `Bite` (گاز, front, very close), `FrontKick` (لگد با پای جلو, front) and `RearKick` (جفتک, behind). The server picks the attack by the target's position relative to the donkey. Each attack has its own range, angle, wind-up, cooldown and readable tell; Shield blocks any one. Spec: `Docs/04_DONKEY_AI.md`.

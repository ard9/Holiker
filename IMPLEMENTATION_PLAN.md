# Implementation Plan

Milestones follow `Docs/12_ROADMAP_AND_ACCEPTANCE.md`.

## M0 — Bootstrap (current)
- [x] Git repo, Unity `.gitignore`, Git LFS rules for binary assets
- [x] Design docs in `Docs/`, enemy renamed Beaver → Donkey
- [ ] Unity 6.3 LTS URP project created and committed (`Assets/`, `Packages/`, `ProjectSettings/`)
- [ ] Folder + assembly structure (`Domain`, `Application`, `Client`, `Server`, `Infrastructure`, `Tests`)
- [ ] Scenes: `Boot`, `MainMenu`, `Match_Swamp_Greybox`
- [ ] `BalanceConfig` ScriptableObject with every value from `03_GAMEPLAY_AND_BALANCE.md`
- [ ] Empty Android build and Linux Dedicated Server build both succeed
- [ ] Package versions locked in `Packages/manifest.json` / `packages-lock.json`

Acceptance: project opens, both builds succeed, package versions recorded, no unjustified preview packages.

## M1 — Offline Core
Greybox map, player controller/camera, NavMesh, Donkey FSM + perception, attack/downed/rescue, Shield/Stun, objectives/exit, match state. Pure-C# Domain logic with EditMode tests.

## M2 — Multiplayer Vertical Slice
## M3 — Backend & Matchmaking
## M4 — Android UX / Optimization
## M5 — Release Candidate

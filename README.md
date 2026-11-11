# Holiker

An online Android chase game: **2–5 penguins 🐧 try to escape a smart donkey 🫏** on a small map. Players collect parts, unlock the exit, use **Shield** and **Stun**, dodge the donkey's bite, front kick and rear kick, and rescue downed teammates before the donkey catches them.

> بازی آنلاین اندرویدی: ۲ تا ۵ پنگوئن باید قطعات را جمع کنند، خروجی را باز کنند و از دست یک خرِ باهوش فرار کنند.

## Stack
- Unity 6.3 LTS · C# · URP · Android
- Netcode for GameObjects + Unity Transport
- Server-authoritative (Linux dedicated server, Host mode for early playtests)
- Unity Gaming Services behind adapters (see `Docs/DECISIONS.md`)

## Docs
All design documents are in [`Docs/`](Docs/). Start with [`Docs/00_MASTER_AI_PROMPT_FA.md`](Docs/00_MASTER_AI_PROMPT_FA.md).

| Doc | Topic |
|---|---|
| `01_GAME_DESIGN_DOCUMENT.md` | Game design |
| `02_TECH_STACK_AND_ARCHITECTURE.md` | Architecture |
| `03_GAMEPLAY_AND_BALANCE.md` | Rules & balance |
| `04_DONKEY_AI.md` | Donkey AI |
| `05`–`13` | Networking, backend, client, security, performance, QA, ops, roadmap, contracts |

## Status
**M0 — Bootstrap** (not started in Unity yet). See [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md).

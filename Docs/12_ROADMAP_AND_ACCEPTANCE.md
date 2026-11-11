# Roadmap & Acceptance

## M0 — Bootstrap
Deliverables: Unity URP project، Android/Linux targets، Git، folder/assembly structure، scenes، Configها، Build خالی هر دو Target.
Acceptance: پروژه باز شود؛ هر دو Build موفق؛ Package versions ثبت؛ Preview بی‌دلیل نباشد.

## M1 — Offline Core
Deliverables: Greybox Swamp، Player Controller/Camera، NavMesh، Donkey FSM/Perception، Attack/Downed/Rescue، Shield/Stun، Objective/Exit، Match State.
Acceptance: یک مسابقه آفلاین از شروع تا پایان انجام شود؛ AI اطلاعات مخفی نداشته باشد؛ قوانین تست شوند.

## M2 — Multiplayer Vertical Slice
Deliverables: Dedicated Server، اتصال 2 Client، Spawn/Despawn، Input command، movement authority، Sync/Interpolation، Donkey AI Server-side، Ability authoritative، Match result، سپس تست 5 Client.
Acceptance: Server واحد وضعیت مشترک؛ Client نتواند Position/Ability/Result را جعل کند؛ 2–5 بازیکن مسابقه را کامل کنند.

## M3 — Backend & Matchmaking
Deliverables: Guest Auth، MPS Session، Quick Match، Private Room، hosting allocation، Cloud Save، Cloud Code result submission، Leaderboard و Remote Config.
Acceptance: Guest ورود کند؛ Private Room ساخته شود؛ نتیجه ذخیره شود؛ duplicate result جایزه دوباره ندهد.

## M4 — Android UX/Optimization
Deliverables: Touch UI، HUD، menu/lobby/results، audio/VFX، localization groundwork، profiling، device matrix، network simulation.
Acceptance: کنترل روی دستگاه مبنا قابل استفاده؛ 30 FPS هدف در بیشتر Match؛ connection loss UX مناسب.

## M5 — Release Candidate
Deliverables: Art pass نقشه، Tutorial، QA pass، Diagnostics، Server deployment، Android release build، privacy/store checklist، runbook.
Acceptance: Bug بحرانی باز نباشد؛ 5 بازیکن آنلاین Match را تمام کنند؛ سرور و ثبت نتیجه امن باشد؛ rollback مستند باشد.

## اولویت
P0: Playable core، server authority، یک map، یک AI، Shield/Stun، Match end.
P1: Matchmaking، profile/result persistence، reconnect، Android optimization.
P2: Cosmetic، leaderboard، maps.
P3: Social، Voice، Ranked، Events، Live Operations.

## Definition of Done
کد Commit شده؛ Unit/Integration test مرتبط نوشته و اجرا شده؛ warning/error جدید بررسی شده؛ authority/security/performance review انجام شده؛ Docs به‌روزرسانی؛ Build/Test status واقعی گزارش شده.

# Testing & QA

## لایه‌های تست
### Unit/EditMode
Match state transitions، Cooldown، Shield rules، Stun range/LOS validation، objective scaling، target scoring، Rage scheduler با Seed، نتیجه Match و Idempotency.

### PlayMode
Movement/Collision، NavMesh/Patrol، Attack/Downed/Rescue، Shield/Stun، Objective/Exit، UI transition و scene lifecycle.

### Integration
Dedicated Server + 2 Client؛ سپس 5 Client؛ Spawn/Despawn، Input/Sync، Session create/join، result save، reconnect و server shutdown.

### Device QA
دستگاه Android مبنا، متوسط و قوی؛ نسبت تصویرها، Touch controls، Thermal/Battery، Wi-Fi و Cellular.

## Test Matrix
| سناریو | انتظار |
|---|---|
| 2 Client | Match شروع شود |
| 5 Client | همه Stateها Sync شوند |
| Client ششم | Join رد شود |
| فقط 1 Client | Match شروع نشود |
| Donkey دید مستقیم | Chase |
| Sprint خارج دید ولی داخل Hearing | Investigate |
| Shield هنگام حمله | یک حمله Block |
| Shield در Cooldown | Server رد کند |
| Stun در Range و LOS | Target موقتاً متوقف |
| Stun خارج Range | رد |
| Rescue | Player به Active برگردد |
| Rescue timeout | Captured |
| همه Captured | Donkey Win |
| Escape | بازیکن Escaped |
| قطع اتصال | Grace/Reconnect طبق Policy |
| Server crash | نتیجه جعلی ثبت نشود |

## Network Simulation
RTT 50/100/150/250ms، packet loss 1%/3%، jitter، قطع اتصال، Wi-Fi↔Cellular، خروج عمدی، version mismatch و RPC flood با Unity Multiplayer Tools یا ابزار معادل بررسی شوند.

## Security QA
Speed hack، teleport، Stun خارج range، Ability در cooldown، Objective خارج محدوده، duplicate collect/result، RPC flood و Token نامعتبر.

## Performance QA
5 بازیکن هم‌زمان، Donkey در Chase/Rage، افکت‌ها، مسیرهای مختلف، چند Match متوالی، Server memory/CPU، GC spike، Network bytes/sec و Android frame time.

## Acceptance
- بدون Bug بحرانی در Match flow
- Clientها اختلاف State پایدار نداشته باشند
- نتیجه Match فقط یک‌بار ذخیره شود
- 5 بازیکن بدون crash وارد و مسابقه را تمام کنند
- Android baseline در بیشتر زمان Match حداقل 30 FPS هدف داشته باشد
- Disconnect باعث قفل دائمی Room نشود
- تمام قابلیت‌های مهم Server-authoritative باشند
- Test/Build اجرا نشده هرگز به‌عنوان موفق گزارش نشود.

## Bug Report
Title، Build، Device/OS، Server Region، MatchId، Steps، Expected، Actual، Frequency، Logs، Video، Severity.

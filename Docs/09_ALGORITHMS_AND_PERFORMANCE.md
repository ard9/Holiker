# Algorithms & Performance

## اصل
هیچ «سریع‌ترین الگوریتم» مطلقی وجود ندارد؛ انتخاب به ورودی، سخت‌افزار و معیار بستگی دارد. MVP باید با ساده‌ترین الگوریتم درست ساخته شود و سپس Profiler تعیین کند کجا بهینه‌سازی لازم است.

## انتخاب‌ها
| مسئله | راهکار | دلیل |
|---|---|---|
| Navigation | Unity NavMesh | نقشه ثابت و 3D |
| Custom pathfinding | A* + Binary Heap در صورت نیاز | مسیر گرافی |
| Donkey behavior | FSM + Utility Scoring | قابل فهم و تست |
| Target selection | اسکن O(P) | فقط 2–5 بازیکن |
| Vision | FOV با Dot Product + Raycast | فیلتر ارزان قبل از LOS |
| Hearing | Sound Event Queue | رویدادمحور |
| Collision | Unity Physics/Capsule | حرکت کاراکتر |
| Network | Server snapshots + interpolation | وضعیت مشترک |
| Match result | Idempotent transaction | جلوگیری از Reward تکراری |

## A*
اگر Pathfinding سفارشی لازم شد، Open Set با Binary Min-Heap، Closed Set، G cost، H heuristic و جلوگیری از بازدید تکراری استفاده شود. پیچیدگی معمول `O((V+E) log V)` برای بخش پیمایش‌شده گراف است و به شکل نقشه و heuristic وابسته است. برای MVP NavMesh انتخاب کم‌ریسک‌تری از بازنویسی Pathfinding است.

## Target scoring
برای حداکثر 5 بازیکن تمام هدف‌های معتبر را بررسی کن:
`score = visibleBonus + recentNoiseBonus + downedPriority - distancePenalty - staleInfoPenalty`
Hysteresis بگذار تا Donkey بی‌دلیل هدف را پیوسته عوض نکند.

## Spatial Hash
در MVP با 5 بازیکن Spatial Hash معمولاً لازم نیست؛ O(P) ساده کافی است. اگر تعداد Entityهای محیطی زیاد شد، Grid/Spatial Hash می‌تواند Query همسایه‌ها را از اسکن همه Entityها کم کند. ابزار را صرفاً برای مدرن‌نمایی اضافه نکن.

## Tick
- Server simulation با Fixed Tick
- UI/Animation در Update/LateUpdate Client
- AI perception و Repath با نرخ کنترل‌شده
- Server monotonic time برای Timer
- Timer مرکزی یا struct سبک به‌جای Coroutineهای بی‌حساب

## Memory/GC
LINQ/Closure غیرضروری در مسیر پرتکرار نساز؛ buffer قابل استفاده مجدد؛ Pooling برای VFX پرتکرار؛ Instantiate/Destroy مکرر نداشته باش؛ allocation در Server Tick اندازه‌گیری شود.

## Android rendering
URP، baked lighting، محدودیت Shadow، LOD، Texture compression، کنترل transparent overdraw و particle count. تست واقعی روی دستگاه مبنا لازم است.

## Budget اولیه
- Android baseline: حداقل 30 FPS پایدار
- دستگاه متوسط/قوی: هدف 60 FPS
- Server Tick: 20Hz
- Snapshot: 10–15Hz نقطه شروع
- Repath: چند بار در ثانیه، نه هر Frame
- CPU/GPU/Memory را پس از انتخاب دستگاه مبنا اندازه‌گیری کن.

## Optimization workflow
1. سناریوی ثابت بساز.
2. روی دستگاه واقعی و Server profile بگیر.
3. CPU/GPU/GC/Network/Tick time را جدا اندازه بگیر.
4. Hotspot را شناسایی کن.
5. یک تغییر انجام بده و دوباره همان تست را اجرا کن.
6. نتیجه را در `PERFORMANCE_REPORT.md` ثبت کن.
Burst/Jobs/ECS فقط در صورت Bottleneck اثبات‌شده وارد شود.

# Donkey AI Specification

## اصول
AI روی Dedicated Server اجرا شود؛ Client فقط وضعیت و Animation لازم را دریافت کند. AI نباید به موقعیت لحظه‌ای همه بازیکنان دسترسی داشته باشد. اطلاعات AI از Vision، Hearing، Last Known Position و وضعیت‌های مجاز می‌آید.

## State Machine
- `Patrol`: رفتن میان Waypointها
- `Investigate`: بررسی صدای معتبر
- `Chase`: تعقیب بازیکنی که دیده شده
- `Search`: جست‌وجوی محدوده LastKnownPosition
- `Attack`: Wind-up، Resolve و Recovery
- `ReturnToPatrol`: بازگشت به مسیر
- Rage یک حالت Overlay است که سرعت/پارامترها را موقتاً تغییر می‌دهد.

## Vision
مقادیر شروع:
- FOV: حدود 100 درجه
- Range: حدود 12 متر
- ابتدا Dot Product برای FOV؛ فقط در صورت داخل دید بودن Raycast/LOS.
- موانع دید را مسدود کنند.
- با گم‌شدن هدف، فقط LastKnownPosition ذخیره شود؛ AI موقعیت جاری هدف را نداند.
- Perception با نرخ کنترل‌شده اجرا شود، نه الزاماً هر Frame.

## Hearing
SoundEvent:
- Position
- Radius
- Loudness
- EventType
- CreatedAtServerTime
- ExpiryTime
- InstigatorId در صورت نیاز و مجاز بودن

شعاع‌های اولیه:
- Walk: 3m
- Sprint: 8m
- Collect/Interact: 6m
- Ability: 9m
- Stun caster cue: 10m

رویدادهای منقضی‌شده حذف شوند. برای MVP یک Queue/Buffer محدود کافی است. قدرت صدا با فاصله کاهش یابد؛ در صورت وجود مانع می‌توان ضریب کاهنده افزود.

## انتخاب Target
برای هر Player معتبر Score ساده محاسبه شود:
`score = visibleBonus + recentNoiseBonus + downedPriority - distancePenalty - staleInfoPenalty`

- Visible target امتیاز بالا دارد.
- صدای تازه/بلند امتیاز می‌دهد.
- فاصله مهم است، اما نباید تنها معیار باشد.
- Hysteresis از تغییر هدف پی‌درپی جلوگیری کند.
- Captured هدف معتبر نیست.
- حداکثر 5 بازیکن است؛ O(P) برای انتخاب هدف کافی است.

## Navigation
- NavMesh در نقشه Bake شود.
- حرکت Server-side با NavMeshAgent یا Motor کنترل‌شده پیاده شود.
- Repath فقط هنگام تغییر معنی‌دار هدف، بسته شدن مسیر یا Timer.
- Chase repath اولیه 0.35 تا 0.6 ثانیه.
- از Pathfinding در هر Frame پرهیز شود.
- موانع Dynamic با NavMeshObstacle/Carving فقط در صورت نیاز.
- مسیرهای نقشه به صورت حلقه‌ای طراحی شوند تا AI و بازیکن در گوشه گیر نکنند.

## Rage
| Setting | مقدار اولیه |
|---|---:|
| Interval | تصادفی 45–75 ثانیه |
| Warning | 1.5 ثانیه |
| Duration | 8 ثانیه |
| Speed multiplier | 1.35 |
- RNG و Timer فقط Server-side.
- Bray (عرعر)، چشم درخشان و VFX پیش از Rage.
- Rage حرکت را سریع‌تر می‌کند، نه اینکه Teleport یا عبور از همه موانع بدهد.

## Attack
خر سه حمله دارد که هر کدام برای یک موقعیت پنگوئن نسبت به خر است:

| Attack | جهت | Range | زاویه | Wind-up | Cooldown | اثر |
|---|---|---:|---:|---:|---:|---|
| `Bite` (گاز) | جلو، خیلی نزدیک | 1.2m | 50° | 0.4s | 1.5s | Downed |
| `FrontKick` (لگد با پای جلو) | جلو | 1.8m | 70° | 0.6s | 2.5s | Downed + Knockback کوتاه |
| `RearKick` (جفتک) | پشت سر | 2.2m | 100° پشت | 0.8s | 3.5s | Downed + Knockback بلند |

- انتخاب حمله در Server: اگر Target پشت خر و در Range جفتک باشد → `RearKick`؛ اگر جلو و خیلی نزدیک → `Bite`؛ اگر جلو و کمی دورتر → `FrontKick`.
- جفتک جلوی استراتژی «همیشه از پشت خر رد شو» را می‌گیرد، اما Wind-up طولانی و خوانا دارد: خر سرش را پایین می‌برد و وزنش را روی پاهای جلو می‌اندازد.
- هر حمله Animation و صدای Wind-up متمایز دارد تا پنگوئن بفهمد کدام حمله در راه است.
- Shield یک حمله از هر نوع را Block می‌کند.
- همه اعداد نقطه شروع‌اند و در Balance Config قرار می‌گیرند.

شرایط: Target در Range/زاویه همان حمله، Status مجاز و Cooldown آن حمله تمام‌شده.
چرخه: `Windup → Resolve → Recovery`.
در Resolve سرور دوباره فاصله/زاویه را برای همان نوع حمله بررسی می‌کند، Shield را چک می‌کند و سپس Block یا Downed (و Knockback) را اعمال می‌کند. Knockback از Collision و NavMesh عبور نمی‌کند.

## Debug
در Debug Server: State، Target، LastKnownPosition، Vision ray، Hearing radius، Path corners، Rage timer، Attack cooldown و Transition reason.

## تست
- Player خارج دید و بی‌صدا پیدا نشود.
- Sprint در Hear radius باعث Investigate شود.
- دید مستقیم باعث Chase شود.
- قطع دید باعث Search در LastKnownPosition شود.
- Rage طبق Seed/Timer تست‌پذیر باشد.
- Attack فقط در Range/Angle/Cooldown معتبر اجرا شود.
- Shield یک Attack را Block کند.
- NavMesh در مسیرهای باریک/اطراف مانع گیر نکند.

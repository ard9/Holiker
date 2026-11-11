# Gameplay & Balance Specification

> همه عددها نقطه شروع برای Playtest هستند. در Config/ScriptableObject یا Remote Config قرار بگیرند، نه Hard-code در چند Script.

## Match Configuration
| Setting | شروع پیشنهادی |
|---|---:|
| Min/Max Players | 2 / 5 |
| Match Duration | 300 sec |
| Lobby Countdown | 8 sec |
| Donkey Count | 1 |
| Server Tick | 20Hz |
| Objectives | بر اساس تعداد بازیکن |

## Objective Scaling
| Players | Parts |
|---|---:|
| 2 | 2 |
| 3 | 3 |
| 4 | 3 |
| 5 | 4 |

قطعات از Spawn pointهای معتبر و با Seed سرور انتخاب شوند. Objective داخل NavMesh غیرقابل دسترس یا در مسیر مسدود ظاهر نشود.

## Player Movement
- Walk: حدود 4.2 m/s
- Sprint: حدود 5.8 m/s
- Sprint duration: حدود 4 ثانیه
- Recovery: مقدار اولیه 3 ثانیه برای شروع بازیابی؛ مقدار نهایی با تست تعیین شود.
- Server سرعت، شتاب، Collision و Stamina را تعیین می‌کند.

## Ability: Shield
- هدف: محافظت از خود در برابر Donkey Attack
- مدت: 2 ثانیه
- Block: حداکثر یک Attack معتبر
- Cooldown: 28 ثانیه
- فقط Player با Status=Active
- Shield مصونیت کلی از همه مکانیک‌های بازی نیست.
- Server زمان شروع/پایان/مصرف و Cooldown را تعیین می‌کند.

## Ability: Stun
- هدف: توقف موقت بازیکن دیگر
- Range: 7 متر
- Line of Sight: الزامی
- Duration: 1.25 ثانیه
- Cooldown: 35 ثانیه
- هدف باید Active و غیر از Caster باشد.
- Server فاصله، LOS، وضعیت، Cooldown و Rate Limit را بررسی کند.
- درخواست تکراری/Target نامعتبر رد شود.
- استفاده از Stun یک Sound Event یا نشانه قابل تشخیص برای Donkey ایجاد کند.
- زنجیره Stun روی یک Target بدون محدودیت مجاز نیست.

## Abilityهای آینده (خارج MVP)
Decoy، Smoke، Dash یا Rescue Boost فقط پس از بالانس نسخه اول.

## Attack / Downed / Rescue
- خر سه حمله دارد: `Bite` (گاز، Wind-up 0.4s)، `FrontKick` (لگد با پای جلو، 0.6s) و `RearKick` (جفتک، 0.8s، برای پنگوئن پشت سر). جزئیات در `04_DONKEY_AI.md`.
- Server در زمان Resolve فاصله/زاویه/Shield را دوباره بررسی می‌کند.
- اگر Shield فعال باشد یک Block مصرف می‌شود.
- در غیر این صورت Active → Downed.
- Downed duration حدود 15 ثانیه.
- Rescue interaction حدود 4 ثانیه.
- حرکت/حمله می‌تواند Rescue را لغو کند.
- پایان Timer بدون Rescue: Captured؛ بازیکن تماشاچی می‌شود و در MVP Respawn ندارد.

## Exit
- بعد از تحویل همه قطعات، خروجی فعال می‌شود.
- Interact/ماندن در ناحیه خروجی حدود 2 ثانیه لازم است.
- هر بازیکن نتیجه شخصی Escape می‌گیرد.
- اگر هیچ‌کس تا اتمام زمان خارج نشود، Donkey Win.

## Fairness
- Rage هشدار بصری/صوتی دارد.
- Attack Wind-up خوانا است.
- Spawn در Line of Sight مستقیم Donkey ممنوع.
- Dead End بی‌راه خروج ممنوع.
- Abilityها Cooldown روشن دارند.
- Stun نباید کنترل را برای زمان طولانی بگیرد.
- سرور RNG را اجرا می‌کند.

## Balance Config
MatchDuration، PlayerWalkSpeed، PlayerSprintSpeed، SprintDuration/Recovery، ShieldDuration/Cooldown، StunRange/Duration/Cooldown، DonkeyPatrol/ChaseSpeed، Bite/FrontKick/RearKick Range/Angle/Windup/Cooldown/Knockback، DownedDuration، RescueDuration، RageInterval/Warning/Duration/Multiplier، ObjectivePartsByPlayerCount.
مقادیر Remote Config در سرور Validate شوند.

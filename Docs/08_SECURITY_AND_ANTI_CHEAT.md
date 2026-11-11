# Security & Anti-Cheat

## Threat model
Speed Hack، Teleport، cooldown bypass، Ability spam، Stun خارج Range/LOS، جعلی‌کردن Objective/Escape/Result، دستکاری XP/Currency، RPC flood، Reconnect abuse و duplicate result.

## Server Authority
Server مالک نهایی Position/Velocity، Stamina، Ability/Cooldown، Attack/Hit، Donkey AI، Objective، Rescue، Match timer/phase، Escape/Capture و Reward است. Client فقط Command می‌فرستد.

## Movement validation
- Move vector در محدوده [-1,1] و طول مجاز clamp شود.
- NaN/Infinity رد شود.
- سرعت/شتاب از Balance Config فراتر نرود.
- Collision و NavMesh/محیط توسط Server محاسبه شود.
- Input sequence تکراری/قدیمی رد شود.
- نرخ ورودی محدود شود.
- Client timestamp مرجع Timer نباشد.

## Ability validation
Shield: Player Active، cooldown پایان‌یافته، match مجاز و درخواست غیرتکراری.
Stun: Caster Active، target معتبر و Active، target غیر از Caster، فاصله Server-side در Range، LOS سمت Server، cooldown تمام و Rate Limit معتبر. Targetهای غیرمجاز/تکراری رد شوند.

## Objective validation
فاصله مجاز، مدت interaction با Server Time، object state، player state و امکان تعامل بررسی شود. هر قطعه فقط یک‌بار Collect شود. Race condition با State transition اتمیک کنترل شود.

## Result/Reward
- نتیجه فقط از Server معتبر پذیرفته شود.
- MatchId یکتا باشد.
- زمان و رویدادها از نظر منطقی بررسی شوند.
- Client حق تعیین XP/Currency ندارد.
- Idempotency Key مانع ثبت دوباره جایزه شود.
- Credential سرویس فقط سمت Server/Secret Store نگهداری شود.

## Secrets
هیچ Secret در APK، ScriptableObject، Addressables یا Git نباشد. محیط‌های Dev/Staging/Production جدا. Token در Log چاپ نشود. Least privilege رعایت شود.

## Abuse prevention
Rate limit RPC و join، Grace Period مشخص، ثبت الگوی غیرطبیعی و سیاست Kick/Ban قابل حسابرسی. در MVP از Ban خودکار با یک سیگنال منفرد و احتمال False Positive بالا پرهیز شود.

## Privacy
فقط داده ضروری جمع‌آوری شود. نام نمایشی با محدودیت طول/کاراکتر Validate شود. در MVP Voice Chat وجود ندارد. Retention، حذف حساب و الزامات Store/کشور انتشار بررسی شود.

## Security tests
Speed غیرممکن، Teleport، Stun خارج Range/پشت دیوار، Shield در cooldown، Collect تکراری/هم‌زمان، MatchResult تکراری، RPC flood، Token نامعتبر و APK secret scan.

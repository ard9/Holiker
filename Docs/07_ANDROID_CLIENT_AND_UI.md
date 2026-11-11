# Android Client، UI و کنترل

## تنظیمات
- Unity 6.3 LTS، URP
- Landscape
- ARM64 و IL2CPP در Release
- Minimum Android API با توجه به Device Matrix و SDK زمان Build تعیین شود؛ حدس نزن.
- Canvas Scaler و Safe Area
- Touch-first UI

## دوربین
Isometric/Angled Top-down؛ دید کافی از شخصیت، مانع و دشمن. Occlusion Fade/Cutaway در صورت نیاز. فاصله دوربین برای نسبت تصویرهای مختلف تست شود. دوربین فقط Presentation است و روی Server State اثر ندارد.

## Controls
- Joystick چپ؛ حرکت 8 جهته و Dead Zone
- Sprint
- Shield
- Stun و هدف‌گیری بازیکن دیگر
- Interact/Rescue
- Pause/Settings
- اندازه/شفافیت کنترل و حساسیت قابل تنظیم

Buttonها Feedback لمس و Cooldown واضح داشته باشند.

## HUD
Timer، Objective progress، Exit state، وضعیت هم‌تیمی‌ها، Ability cooldown، Interact prompt، Downed/Rescue پیام و Network quality در صورت امکان. HUD نباید از اطلاعات پنهان AI استفاده کند.

## Flow
Boot → Initialize Services → Guest/Auth → Main Menu → Quick Match/Private Room → Lobby/Ready → Match → Results.
خطاهای Auth، Matchmaking timeout، Room full، Allocation failure، Network lost، Reconnecting، Version mismatch و Save pending پیام کاربرپسند داشته باشند.

## Localization
متن ثابت داخل Scriptها نباشد؛ Localization system استفاده شود. فارسی و انگلیسی قابل افزودن باشند. RTL، عدد، Timer و Text overflow تست شود.

## Android Performance
URP، Baked lighting، محدودیت Real-time shadows/lights، LOD، Texture Compression مناسب، کنترل Overdraw و Particle count، Object Pooling برای VFX تکراری. فقط در صورت سود واقعی Occlusion Culling. تست دستگاه واقعی ضروری است.

## Art Direction
شخصیت‌ها Stylized و Silhouette خوانا. رنگ Shield، Stun و Rage از هم متمایز. Donkey در حالت عادی و Rage سریع قابل تشخیص باشد. UI کوتاه و قابل لمس طراحی شود.

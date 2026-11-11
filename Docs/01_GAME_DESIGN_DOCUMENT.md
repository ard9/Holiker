# GDD — Holiker

## خلاصه
- پلتفرم: Android
- ژانر: Multiplayer Survival / Chase / Party
- ظاهر: 3D Stylized Cartoon، کمی مرموز ولی غیرواقع‌گرایانه
- بازیکنان: پنگوئن‌ها؛ دشمن: یک خر باهوش
- دوربین: Isometric / Angled Top-down
- مسابقه: 2 تا 5 بازیکن در برابر یک Donkey AI
- مدت هدف: 4–6 دقیقه
- نام موقت: Holiker

## Fantasy
بازیکنان در نقشه کوچکی گرفتار شده‌اند. خر با گشت‌زنی، دیدن و شنیدن آن‌ها را پیدا می‌کند. بازیکنان باید قطعات لازم را پیدا کنند، خروجی را باز کنند، از Abilityها استفاده کنند و در صورت امکان هم‌تیمی زمین‌گیرشده را نجات دهند.

## ستون‌های طراحی
1. خطر قابل‌فهم؛ بازیکن باید بفهمد چرا AI او را پیدا کرده است.
2. مسابقات کوتاه و تکرارپذیر.
3. AI منصفانه و بدون اطلاع از موقعیت مخفی.
4. حرکت معنادار؛ مخفی شدن دائمی نباید استراتژی غالب باشد.
5. همکاری همراه با خرابکاری محدود از طریق Stun.
6. کنترل موبایلی ساده و خوانا.

## Core Loop
Queue → Lobby → Ready → Spawn → Explore → Collect → Chase/Hide/Ability → Unlock Exit → Escape/Capture → Results.

## Match
- 2 تا 5 بازیکن در یک نقشه
- یک Donkey AI در سرور
- جمع‌آوری قطعات
- فعال شدن خروجی
- خروج هر بازیکن به‌صورت جداگانه
- Downed/Rescue/Captured
- اگر هیچ بازیکنی تا اتمام زمان Escape نکند، Donkey برنده است.

## نقشه MVP
یک Greybox با وسعت تقریبی 45×45 تا 60×60 متر؛ باتلاق، پل چوبی، بوته، ساختمان کوچک، موانع دید، 3–4 مسیر حلقه‌ای، چند فضای باز و خروجی روشن. هیچ نقطه مخفی دائمی یا Dead End بی‌راه خروج نباشد.

نقشه‌های بعدی:
- Abandoned Factory
- Flooded Village
- Night Forest
هر نقشه باید مکانیک متمایز داشته باشد، نه فقط Texture متفاوت.

## کنترل
Landscape؛ Joystick چپ، دکمه Sprint، Shield، Stun، Interact/Rescue و Pause. اندازه کنترل‌ها و حساسیت باید قابل تنظیم باشد.

## شخصیت/موجودیت
PlayerCharacter، DonkeyEnemy، ObjectivePart، ExitGate، RescueInteractable، SoundEvent، MapHazard.

## Player States
Spawning، Active، Stunned، Shielded، Downed، Captured، Escaped، Disconnected.

## برد/باخت
- Escape: بازیکن از خروجی عبور کند.
- Captured: Downed شده و نجات نشده باشد.
- Donkey Win: هیچ بازیکنی تا پایان Match خارج نشود یا همه Captured شوند.
- هر بازیکن نتیجه شخصی Escape/Captured می‌گیرد.

## UI
Boot، Login/Guest، Main Menu، Quick Match، Private Room، Lobby، Match HUD، Pause/Settings، Results، Profile.

HUD: Timer، هدف‌ها، وضعیت هم‌تیمی‌ها، Cooldown Ability، Interact prompt، هشدار Donkey فقط در صورت اطلاعات مجاز.

## صدا و بازخورد
قدم Walk/Sprint، جمع‌آوری قطعه، تعامل، Ability، Bray (عرعر) پیش از Rage و Attack Wind-up. جهت صدای UI نباید اطلاعاتی فراتر از قوانین Perception آشکار کند.

## پیشرفت
در MVP فقط XP و آمار پایه. در آینده Cosmetic/Emote/Badge/Title. هیچ Pay-to-Win وجود نداشته باشد.

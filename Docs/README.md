# بسته توسعه Holiker

این بسته برای تحویل به یک عامل برنامه‌نویس هوش مصنوعی آماده شده است. ابتدا `00_MASTER_AI_PROMPT_FA.md` را به عامل بدهید و تمام فایل‌های دیگر را نیز در اختیارش قرار دهید.

## تصمیمات پایه
- Android؛ Unity 6.3 LTS؛ C#؛ URP
- Multiplayer: Netcode for GameObjects (NGO) + Unity Transport
- Session/Matchmaking: Unity Multiplayer Services SDK
- یک Dedicated Server لینوکسی برای هر Match؛ Server-authoritative
- Backend: Unity Gaming Services (Authentication, Cloud Save, Cloud Code, Leaderboards, Remote Config)
- هر مسابقه: 2 تا 5 بازیکن + یک خر AI
- MVP: یک نقشه، دو Ability اصلی، اهداف جمع‌آوری، خروج، Downed/Rescue/Captured

## فایل‌ها
- `00_MASTER_AI_PROMPT_FA.md`: پرامپت اصلی عامل برنامه‌نویس
- `01_GAME_DESIGN_DOCUMENT.md`: سند طراحی بازی
- `02_TECH_STACK_AND_ARCHITECTURE.md`: معماری و فناوری
- `03_GAMEPLAY_AND_BALANCE.md`: قوانین و بالانس
- `04_DONKEY_AI.md`: هوش مصنوعی خر
- `05_MULTIPLAYER_NETWORKING.md`: شبکه و همگام‌سازی
- `06_BACKEND_AND_DATA.md`: سرویس‌ها و دیتابیس
- `07_ANDROID_CLIENT_AND_UI.md`: کلاینت اندروید و رابط کاربری
- `08_SECURITY_AND_ANTI_CHEAT.md`: امنیت و ضدتقلب
- `09_ALGORITHMS_AND_PERFORMANCE.md`: الگوریتم‌ها و کارایی
- `10_TESTING_AND_QA.md`: تست
- `11_BUILD_DEPLOYMENT_AND_OPERATIONS.md`: Build و استقرار
- `12_ROADMAP_AND_ACCEPTANCE.md`: نقشه راه
- `13_NETWORK_CONTRACTS_AND_DATA_MODELS.md`: قراردادهای داده و شبکه

## نکته فنی
«سریع‌ترین زبان» بدون توجه به نوع کار، سخت‌افزار و معماری معیار معناداری نیست. برای این بازی، C# در Unity انتخاب متوازن است؛ سرور authoritative، الگوریتم مناسب، تعداد پیام‌ها، مسیر‌یابی و رندر موبایل اثر بیشتری بر Performance دارند. Go/Rust/Redis/PostgreSQL را بدون نیاز اثبات‌شده به MVP اضافه نکنید.

## منابع رسمی
- https://unity.com/releases/unity-6/support
- https://docs.unity.com/en-us/multiplayer/netcode/netcode
- https://docs.unity.com/en-us/mps-sdk/index
- https://docs.unity.com/en-us/multiplayer/quickstarts/competitive-quickstart
- https://docs.unity.com/en-us/mps-sdk/game-server-hosting-support

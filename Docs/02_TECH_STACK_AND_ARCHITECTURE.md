# Tech Stack & Architecture

## پشته پیشنهادی
| لایه | فناوری | دلیل |
|---|---|---|
| Engine | Unity 6.3 LTS | اکوسیستم Android و LTS |
| Render | URP | بودجه گرافیکی موبایل |
| Language | C# پایدار مورد پشتیبانی Unity | یکپارچگی کلاینت/سرور |
| Multiplayer | Netcode for GameObjects | مناسب GameObject workflow و Room کوچک |
| Transport | Unity Transport | لایه انتقال NGO |
| Session/Matchmaking | Unity Multiplayer Services SDK | Session و matchmaking |
| Identity | Unity Authentication | Guest/Player ID |
| Persistence | Cloud Save | پروفایل و داده کم‌حجم |
| Server Operations | Cloud Code + Dedicated Server | منطق امن و نتیجه Match |
| Ranking | Leaderboards | رتبه‌بندی |
| Tuning | Remote Config | تغییر بالانس |
| Hosting | Adapter روی یک Provider منتخب | قابلیت تعویض Host |

## چرا C#؟
C# هم برای کلاینت و هم Dedicated Server به Unity، Physics، NavMesh، Animation و ابزارها دسترسی مستقیم می‌دهد. Go/Rust برای MVP ارزش افزوده قطعی ندارند و یک پیاده‌سازی موازی ایجاد می‌کنند. اگر بعدها Backend سفارشی یا گلوگاه اندازه‌گیری‌شده وجود داشت، زبان جداگانه با ADR انتخاب شود.

## معماری
```text
Android Unity Client
  ├─ Input/UI/Camera/Audio/VFX
  └─ NGO Client
        │ Input Commands / Replicated State
        ▼
Linux Unity Dedicated Server
  ├─ Match Lifecycle
  ├─ Movement Authority
  ├─ Donkey AI
  ├─ Combat / Ability
  ├─ Objectives / Exit
  └─ Result Validation
        │
        ├─ Unity Multiplayer Services (Session/Matchmaking)
        └─ Secure Persistence
               └─ UGS: Auth / Cloud Save / Cloud Code / Leaderboards
```

## مسئولیت‌ها
Client: نمایش و ارسال ورودی؛ هیچ مرجع نهایی وضعیت بازی نیست.
Server: موقعیت، سرعت، AI، Ability، Combat، Objective، Timer و نتیجه.
Backend: حساب، داده پایدار، Matchmaking، آمار، تنظیمات و رتبه‌بندی.

## ساختار پوشه پیشنهادی
```text
Assets/_Project/
  Art/ Audio/ Prefabs/ Scenes/ ScriptableObjects/
  Scripts/
    Domain/{Match,Player,Abilities,Objectives,AI}
    Application/
    Client/{Input,Camera,UI,Presentation,Audio}
    Server/{Match,Movement,AI,Combat,Objectives,Validation}
    Infrastructure/{Networking,UGS,Persistence,Logging}
  Tests/{EditMode,PlayMode,Integration}
Packages/ ProjectSettings/ Build/ Docs/
```
Assemblyهای پیشنهادی: Domain، Application، Client، Server، Infrastructure، Tests. Domain تا حد امکان مستقل از UnityEngine باشد.

## اصول
- Server authoritative
- یک منبع واحد برای Balance Config
- DTOهای شبکه نسخه‌دار
- Hosting و Persistence پشت Interface
- Match state در حافظه سرور؛ در هر Tick به DB ننویس
- نتیجه با MatchId/Idempotency Key یکتا ثبت شود
- Secret در APK نباشد
- Microservice از روز اول نساز

## چه زمانی Stack را عوض کنیم؟
- Netcode for Entities: فقط اگر مقیاس/Profiling نشان داد NGO گلوگاه است.
- Photon Fusion: اگر قابلیت‌های Prediction/Networking آن نسبت به هزینه و وابستگی بیرونی توجیه داشته باشد.
- PostgreSQL: نیاز رابطه‌ای، گزارش‌گیری سنگین یا استقلال از UGS.
- Redis: نیاز واقعی Cache/Queue در مقیاس بالاتر.
- Go/Rust: سرویس مستقل با Benchmark و مرز API روشن.
هر تغییر باید ADR و برنامه مهاجرت داشته باشد.

## منابع
- Unity 6: https://unity.com/releases/unity-6/support
- NGO: https://docs.unity.com/en-us/multiplayer/netcode/netcode
- MPS: https://docs.unity.com/en-us/mps-sdk/index
- Competitive quickstart: https://docs.unity.com/en-us/multiplayer/quickstarts/competitive-quickstart
- Hosting support: https://docs.unity.com/en-us/mps-sdk/game-server-hosting-support

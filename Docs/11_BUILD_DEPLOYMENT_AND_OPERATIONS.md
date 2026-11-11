# Build, Deployment & Operations

## Environments
Development، Staging و Production جدا باشند؛ Credentials، UGS environment، Remote Config، Logs و داده تست تفکیک شوند.

## Android Build
- Android Build Profile
- IL2CPP و ARM64 برای Release
- Release signing از Secret Store
- APK برای تست داخلی؛ AAB برای Google Play در صورت نیاز
- Build number/version/environment ثبت شود.
- Minimum API Level با توجه به Device Matrix و الزامات فعلی Store انتخاب شود.

## Linux Dedicated Server
- Dedicated Server Build Target
- Headless و حذف UI/Audio/Textureهای client-only
- Log ساختاریافته
- Health/Ready signal
- پورت و Transport از Config/Environment
- graceful shutdown
- ظرفیت هر Match حداکثر 5 بازیکن

## Hosting Adapter
Interface برای Allocate، Endpoint، Server Authentication، Register/Publish Session، Deallocate و Health status.
Provider پس از مقایسه Linux support، Region، قیمت، Cold start، Autoscaling، Port allocation، Matchmaker integration، Metrics، SLA و قابلیت خروج از سرویس انتخاب شود. Unity Multiplayer Services قابلیت اتصال به Hosting Providerهای مختلف را از مسیر Cloud Code Modules مستند کرده است؛ Integration دقیق به Provider منتخب وابسته است.

## CI Pipeline
Checkout → Restore/verify packages → Static checks → EditMode tests → PlayMode tests → Android build → Linux server build → Upload artifacts → Staging deploy with approval → Smoke tests.
Secret در YAML/Logs قرار نگیرد.

## Logging/Metrics
Events: ServerStart/Ready/Shutdown، MatchCreated/Started/Ended، PlayerJoined/Left/Reconnected، AbilityAccepted/Rejected، DonkeyStateChanged، ObjectiveCollected، PlayerDowned/Rescued/Captured/Escaped، ResultPersisted/Failed.
Metrics: CPU/Memory، match count، players/server، average RTT، disconnect rate، allocation failures، result failures و crash rate. Secret/PII غیرضروری Log نشود.

## Server lifecycle
Server پس از allocation معتبر Ready شود؛ session metadata ثبت شود؛ پس از Match result ذخیره و منابع آزاد شوند. Crash نباید به Win/Loss عادی تبدیل شود.

## Rollback
Build قبلی Android و Server حفظ شود؛ Client/Server build compatibility check؛ Remote Config rollback؛ migration داده backward-compatible؛ طرح بازگردانی Staging/Production مستند شود.

## Store checklist
Package name، icon، screenshots، privacy policy، Data Safety، permission review، content rating، signing backup، crash/ANR monitoring، network error UX، deletion requirements و تست Release build روی دستگاه واقعی.

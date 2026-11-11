# Backend، خدمات و دیتابیس

## اصل معماری
- Live Match State در حافظه Dedicated Server است.
- داده دائمی بازیکن در UGS Cloud Save یا سرویس انتخابی ذخیره می‌شود.
- Client اجازه تغییر مستقیم داده حساس ندارد.
- نتیجه/XP/Stats از مسیر امن Server/Cloud Code ثبت می‌شود.
- برای MVP دیتابیس SQL مستقل اجباری نیست؛ Cloud Save یک سرویس ذخیره‌سازی مدیریت‌شده است.
- PostgreSQL فقط اگر نیاز واقعی Query رابطه‌ای، گزارش‌گیری، استقلال از UGS یا داده اجتماعی ایجاد شد اضافه شود.

## سرویس‌های پیشنهادی UGS
### Authentication
Guest/Anonymous در شروع؛ امکان Link کردن Identity Provider بعداً. Session token فقط با SDK رسمی مدیریت شود.

### Multiplayer Services SDK
Sessions، Player grouping، Quick Join/Matchmaker، Private Room/Join Code و اتصال به Server. از SDK یکپارچه استفاده شود، نه APIهای مستقل قدیمی بدون دلیل.

### Cloud Save
برای Profile، Settings، Progression، Statistics و Cosmetic unlocks. داده حساس فقط با مسیر دسترسی Server/Cloud Code تغییر کند.

### Cloud Code
اعتبارسنجی Match Result، محاسبه XP/Reward، ثبت Stats و تغییرات Economy. Client هرگز Admin/Service Account Credential نداشته باشد.

### Leaderboards
Escapes، Matches played، Rescues، Seasonal score. عددها از رویدادهای معتبر Server ساخته شوند.

### Remote Config
Match duration، Ability cooldown، Donkey speed، Rage interval، Objective count و Feature flags. سرور مقادیر را Validate کند.

### Economy (پس از MVP)
Cosmetic currency، Cosmetic Inventory و Purchase definitions. هیچ Pay-to-Win وجود نداشته باشد.

### Analytics/Diagnostics
MatchStarted/Ended، Escape/Capture/Rescue، AbilityUsed، RageActivated، Disconnect و Crash. Analytics نباید Match loop را Block کند.

## چه چیزی را ذخیره کنیم؟
### فقط در حافظه Match
Player position/velocity، stamina، cooldown، Donkey state/path/target، sound events، objective states، match timer و combat state. این موارد در هر Tick وارد دیتابیس نشوند.

### Persistent Player Data
displayName، avatar/cosmetic، XP/level، lifetime stats، settings و unlocks.

### Match Summary
MatchId، MapId، BuildVersion، PlayerIds، Duration، EndReason، outcome هر Player، objectives، rescues، captures، escapes، createdAt. مختصات لحظه‌ای در MVP ذخیره نشوند.

## Cloud Save Keys
`profile_v1`، `progression_v1`، `statistics_v1`، `settings_v1`، `cosmetics_v1`. هر Document دارای schemaVersion باشد؛ Migration/Backward compatibility تعریف شود.

## مدل منطقی
- PlayerProfile(playerId, displayName, avatarId, createdAt, schemaVersion)
- PlayerProgression(playerId, xp, level, softCurrency, schemaVersion)
- PlayerStatistics(playerId, matchesPlayed, escapes, captures, rescues, wins, losses, schemaVersion)
- PlayerSettings(playerId, language, volumeSettings, controlScale, cameraSensitivity, schemaVersion)
- MatchSummary(matchId, mapId, buildVersion, startedAt, endedAt, endReason, participants, schemaVersion)

## طرح PostgreSQL برای آینده (نه الزام MVP)
```sql
CREATE TABLE players (
  player_id TEXT PRIMARY KEY,
  display_name VARCHAR(32) NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  schema_version INT NOT NULL DEFAULT 1
);
CREATE TABLE player_stats (
  player_id TEXT PRIMARY KEY REFERENCES players(player_id),
  matches_played BIGINT NOT NULL DEFAULT 0,
  escapes BIGINT NOT NULL DEFAULT 0,
  captures BIGINT NOT NULL DEFAULT 0,
  rescues BIGINT NOT NULL DEFAULT 0,
  wins BIGINT NOT NULL DEFAULT 0,
  losses BIGINT NOT NULL DEFAULT 0,
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
CREATE TABLE matches (
  match_id UUID PRIMARY KEY,
  map_id TEXT NOT NULL,
  build_version TEXT NOT NULL,
  started_at TIMESTAMPTZ NOT NULL,
  ended_at TIMESTAMPTZ NOT NULL,
  end_reason TEXT NOT NULL,
  schema_version INT NOT NULL DEFAULT 1
);
CREATE TABLE match_players (
  match_id UUID NOT NULL REFERENCES matches(match_id),
  player_id TEXT NOT NULL REFERENCES players(player_id),
  outcome TEXT NOT NULL,
  objectives_collected INT NOT NULL DEFAULT 0,
  rescues INT NOT NULL DEFAULT 0,
  PRIMARY KEY (match_id, player_id)
);
```
قبل از Production، تراکنش اتمیک، Index، Retention و Privacy بررسی شود.

## ثبت امن نتیجه
1. Server Match Summary را بسازد.
2. MatchId و Server identity بررسی شود.
3. Idempotency Key یکتا ارسال شود.
4. Cloud Code/Backend نتیجه و Reward را Validate کند.
5. Stats و Leaderboard به‌روزرسانی شوند.
6. Retry همان MatchId نباید Reward را دوباره اضافه کند.

## خطا و Rate Limit
Rate limit برای endpointها، Retry با Exponential Backoff/Jitter فقط برای خطاهای transient، مدیریت Cloud Save conflict، عدم Log کردن Secret و مسیر امن برای Retry ذخیره نتیجه.

# Network Contracts & Data Models

> این‌ها قراردادهای منطقی‌اند. Agent آن‌ها را به DTOهای strongly typed و API سازگار با NGO Package نصب‌شده تبدیل کند؛ نام دقیق Attribute/RPC باید از مستندات همان نسخه بررسی شود.

## Versioning
پیام‌ها و Match دارای protocolVersion/buildVersion باشند. ناسازگاری Client/Server قبل از ورود به بازی رد شود.

## Client → Server

### PlayerInputCommand
```json
{
  "protocolVersion": 1,
  "sequence": 1024,
  "moveX": 0.0,
  "moveY": 1.0,
  "sprint": true,
  "buttons": 0,
  "clientTick": 650
}
```
Client position نفرستد. Move axis به [-1,1] clamp شود. NaN/Infinity و sequence قدیمی/تکراری رد شوند. Server time مرجع باشد.

### AbilityRequest
```json
{
  "protocolVersion": 1,
  "requestId": "unique-request-id",
  "abilityId": "shield",
  "targetNetworkId": null
}
```
برای Stun، targetNetworkId لازم است. requestId از پردازش دوباره جلوگیری کند.

### InteractRequest
```json
{
  "protocolVersion": 1,
  "requestId": "unique-request-id",
  "targetObjectId": "objective_03",
  "interactionType": "collect"
}
```
سرور فاصله، وضعیت Player/Object و امکان تعامل را بررسی کند.

## Server → Client
Player replicated state: NetworkId، Position، Rotation، MovementMode، PlayerStatus، LastProcessedInputSequence، ShieldActive و زمان پایان Effect/Cooldown لازم.
Donkey replicated state: NetworkId، Position، Rotation، CurrentState، RageActive و Cueهای قابل مشاهده. Target پنهان AI بی‌دلیل افشا نشود.
Match state: MatchId، MatchPhase، ServerTime، RemainingTime، ObjectiveCount، RequiredCount، ExitState، player counts و EndReason.

## Match Result DTO
```json
{
  "schemaVersion": 1,
  "matchId": "uuid",
  "mapId": "swamp_mvp",
  "buildVersion": "0.1.0",
  "startedAtUtc": "ISO-8601",
  "endedAtUtc": "ISO-8601",
  "endReason": "escaped",
  "participants": [
    {
      "playerId": "service-player-id",
      "outcome": "escaped",
      "rescues": 1,
      "objectivesCollected": 1,
      "captured": false
    }
  ]
}
```
Result فقط Server/Backend معتبر باشد؛ نمونه قرارداد مفهومی است و باید از رویدادهای authoritative ساخته شود.

## Persistent models
- PlayerProfile: playerId، displayName، avatarId، createdAt، schemaVersion
- PlayerProgression: xp، level، softCurrency، schemaVersion
- PlayerStatistics: matchesPlayed، escapes، captures، rescues، wins، losses، schemaVersion
- PlayerSettings: language، volumes، controlScale، cameraSensitivity، schemaVersion

## Match events
MatchCreated، PlayerJoined، PlayerReadyChanged، MatchStarted، SoundEmitted، DonkeyStateChanged، AbilityRequested/Accepted/Rejected، ObjectiveCollected، ExitUnlocked، PlayerDowned/Rescued/Captured/Escaped، MatchEnded، ResultPersisted.
Eventها MatchId و ServerTime داشته باشند؛ Log پرتعداد فقط با نیاز مشخص ذخیره شود.

## Error codes
SESSION_FULL، MATCH_NOT_FOUND، VERSION_MISMATCH، PLAYER_NOT_AUTHENTICATED، PLAYER_NOT_ACTIVE، ABILITY_ON_COOLDOWN، TARGET_OUT_OF_RANGE، TARGET_NOT_VISIBLE، INVALID_INPUT، RATE_LIMITED، MATCH_ALREADY_ENDED، SERVER_UNAVAILABLE، RESULT_ALREADY_PROCESSED.
Client کدها را به پیام کاربرپسند تبدیل کند.

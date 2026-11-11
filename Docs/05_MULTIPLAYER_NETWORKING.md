# Multiplayer Networking

## Topology
Dedicated Server / Client-Server:
- یک Server مستقل برای هر Match
- 2 تا 5 Client
- Server مرجع Player state، Donkey AI، Ability، Objective، Timer و Result
- Client هیچ‌وقت Host معتبر Match نیست.

## داده‌های Server-authoritative
Position/Velocity، Stamina، Sprint state، Shield/Stun/Cooldown، Donkey state، Attack/Downed/Rescue/Capture/Escape، Objective و Match phase/time/result.

Client فقط Input/Command و درخواست Interaction/Ability می‌فرستد.

## Tick و Snapshot اولیه
- Server simulation tick: 20Hz
- Replication snapshot: 10–15Hz
- Client interpolation buffer: حدود 100ms
- Timer با Server Time محاسبه شود.
این نرخ‌ها نقطه شروع هستند؛ با Network Profiler و تست واقعی تنظیم شوند.

## Movement Flow
1. Client Input Command با Sequence می‌فرستد.
2. Server ورودی را clamp و validate می‌کند.
3. Server حرکت را با سرعت و Collision مجاز محاسبه می‌کند.
4. Server State را replicate می‌کند.
5. Client برای بازیکنان دیگر interpolation دارد.
6. در صورت نیاز prediction سبک با LastProcessedInputSequence و reconciliation نوشته شود.
7. اگر Prediction در MVP پیچیده شد، ابتدا Server Authority + Interpolation اجرا شود؛ Client position مرجع نیست.

نکته: NGO را بدون بررسی API نسخه نصب‌شده دارای rollback/prediction کامل فرض نکن.

## NGO و RPC
- از NGO RPC/NetworkVariable مطابق نسخه نصب‌شده استفاده کن.
- Inputهای پرتکرار کوچک باشند.
- Stateهای کم‌تغییر با replication مناسب sync شوند.
- Eventهای یک‌باره مانند Rage cue با رویداد مناسب پخش شوند.
- کل World State را هر پیام نفرست.
- RPC payload و rate limit کنترل شود.

## Match Lifecycle
`Created → WaitingForPlayers → ReadyCheck → Starting → InProgress → Ending → Results → Closed`
- ظرفیت 5
- شروع با حداقل 2 بازیکن
- Private Room و Quick Match
- Build version سازگار
- Join-in-progress در MVP غیرفعال، به جز reconnect همان بازیکن
- Disconnect: Grace Period اولیه 20 ثانیه؛ بعد Disconnected و آزادسازی Slot طبق Policy
- Server crash نباید برد/باخت عادی ثبت کند.

## Reconnect
- Ticket/Identity امن
- حفظ State در Grace Period
- پس از اتصال، Snapshot کامل از Server
- Client اجازه انتخاب Position قبلی ندارد
- پس از اتمام Grace Period ورود مجدد به همان Match رد شود.

## Matchmaking metadata
buildVersion، region، mapPool، minPlayers=2، maxPlayers=5، partyId، gameMode=EscapeRun.

## تست شبکه
RTT 50/100/150/250ms، Packet loss 1%/3%، Jitter، Wi-Fi ↔ Cellular، قطع 5–20 ثانیه، خروج عمدی، Server crash، Version mismatch و RPC flood.

## Metrics
MatchId، ServerId، Region، BuildVersion، PlayerCount، Average RTT، Packet loss در صورت دسترسی، DisconnectReason، MatchDuration و EndReason. Token و اطلاعات شخصی را Log نکن.

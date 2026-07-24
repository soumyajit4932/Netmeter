# Net Meter

Live network speed in the status bar, and a straight answer to where your data went — what used it, when, and whether you were even looking at the screen.

---

## What, when, why

**What** — every app that moved data, sorted by total or by background usage, with a mobile/Wi-Fi split on each. Tap any app for its own screen.

**When** — an hour-by-hour chart of the day, so a 3 a.m. spike is impossible to miss. Plus an activity log: while the meter runs it takes a reading every minute, compares it with the last one, and writes down who moved what. You get entries like *7:41 pm — YouTube, 84 MB, background*. A week of history is kept.

**Why** — this is where honesty matters. Android will tell you whether an app was **on screen** or **in the background** when it used data. It will never tell you what the data contained. So the app gives you the signal that actually answers the question in practice: background traffic with the app closed is sync, ads, auto-updates or backups — and that's the number worth watching. The "While you weren't looking" card puts that percentage front and centre and names the top three offenders.

**Live** — a rolling 60-second trace, download above the centre line, upload mirrored below, with the current speed on each channel.

**History** — 12 months of totals with the same mobile/Wi-Fi split.

---

## Building it

You need Android Studio (Ladybug or newer) on a PC or Mac. Nothing here builds on a phone.

1. Unzip the folder.
2. Android Studio → **Open** → select the `NetMeter` folder.
3. Let it sync. If it asks to download a Gradle wrapper or SDK components, say yes.
4. Plug in your phone with USB debugging on, then press **Run**.

For a shareable file: **Build → Build Bundle(s)/APK(s) → Build APK(s)**. It lands in `app/build/outputs/apk/debug/`.

Targets Android 10 and up. No third-party libraries, no network calls — nothing leaves the phone.

---

## First run

Two switches, both one-time:

- **Usage access** — the app shows an orange card with an "Open settings" button. Find Net Meter in that list and turn it on. Without it, Android returns nothing for the per-app figures.
- **Notifications** — prompted the first time you turn the status bar meter on.

Then leave the meter running for a day. The totals and the hourly chart come from Android's own records and are there immediately; the activity log has to build itself up, so it starts empty.

---

## Things worth knowing

- The status bar shows the combined figure (down + up), because there's only room for one number. The split is in the notification title and on the live panel.
- **The activity log only fills while the meter is running**, plus one reading each time you open the app. Turn the meter off and you'll have gaps.
- Android stores traffic in blocks rather than second by second, so a block straddling two hours gets divided between them. The shape of the day is right; the edges are approximate.
- Detailed per-app records only go back so far (often about 90 days). Older months still show correct totals.
- Xiaomi, Realme, Oppo and Vivo kill background services aggressively. If the meter stops on its own: Settings → Battery → Net Meter → unrestricted, and lock the app in the recents screen.
- Some skins (MIUI especially) replace notification icons with their own. If the number doesn't appear up top, that's the OEM. Those phones usually have a built-in "network speed indicator" in Settings that works better.
- Traffic counters reset on reboot, so today's totals come from Android's records, not from the live sampler.

---

## Layout

```
app/src/main/java/com/ankit/netmeter/
  MainActivity.kt    Compose UI — live panel, hourly chart, app list, detail screen
  SpeedService.kt    Foreground service: 1s speed tick, 60s activity sampler
  ActivityStore.kt   SQLite log — diffs per-app totals to build the timeline
  SpeedIcon.kt       Renders the speed text into the status bar icon bitmap
  NetStatsRepo.kt    NetworkStatsManager queries, foreground/background split
  Format.kt          Byte and speed formatting
  Theme.kt           Colour and type tokens
  Prefs.kt           Remembers whether the meter was on
  BootReceiver.kt    Restarts it after a reboot
```

Change the accents in `Theme.kt` — `Ink.Down` (teal) and `Ink.Up` (ember) drive every bar, dot and readout.

Tuning knobs in `ActivityStore.kt`: `MIN_BYTES` (how small a burst gets logged, default 128 KB) and `KEEP_MS` (how long history is kept, default 7 days). Sampling interval is `ACTIVITY_INTERVAL_MS` in `SpeedService.kt`.

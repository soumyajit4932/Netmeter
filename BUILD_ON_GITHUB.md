# Getting the APK onto your phone — no computer needed

The project is set up so GitHub builds the APK for you in the cloud. You do the whole thing from your phone's browser. Budget about 15 minutes the first time; most of it is GitHub grinding away on its own.

---

## One time: put the code on GitHub

1. Make a free account at **github.com** if you don't have one.
2. Tap **+** (top right) → **New repository**. Name it `netmeter`, leave it Public or Private, tap **Create repository**.
3. On the new repo's page, tap **uploading an existing file** (the link in the middle).
4. Unzip `NetMeter.zip` on your phone first (Files app → tap the zip → Extract). Then upload the **contents** of the NetMeter folder — everything: `app`, `gradle`, `gradlew`, `build.gradle.kts`, `settings.gradle.kts`, the `.github` folder, all of it.
   - The `.github` folder is what tells GitHub to build. If your file manager hides folders starting with a dot, turn on "show hidden files" so it gets uploaded.
5. Scroll down, tap **Commit changes**.

---

## Build it

1. On the repo page, tap the **Actions** tab.
2. If it asks to enable workflows, say yes.
3. You'll see **Build APK**. Every time you upload code it runs on its own, but you can also tap it → **Run workflow** → **Run workflow** to start one.
4. Wait. A yellow dot means building, green tick means done. Usually 3–6 minutes.

---

## Get the file

1. Tap the finished (green) run.
2. Scroll to the bottom, under **Artifacts** there's **NetMeter-debug-apk**. Tap it — it downloads a zip.
3. Extract that zip → inside is `app-debug.apk`.
4. Tap the APK to install. Android will warn about "installing from unknown sources" — allow it for your browser or Files app, then install.

That APK is the real app, signed with a debug key. It installs and runs like any other. It just can't go on the Play Store as-is, which doesn't matter for your own phone.

---

## If the build fails

Tap the red run, then the **build** job, and look for the red step to see the message. The usual causes:

- **The `.github` folder didn't upload** — no runs appear at all in Actions. Re-upload, making sure hidden files are shown.
- **Something didn't upload** — a missing file error near the start. Easiest fix: delete the repo and re-upload the whole folder contents again.

The build regenerates the Gradle wrapper automatically, so you don't need to worry about that part.

---

## Next time you change something

Upload the changed file to the repo (tap the file → pencil icon, or upload again), commit, and Actions rebuilds a fresh APK by itself. Download it the same way.

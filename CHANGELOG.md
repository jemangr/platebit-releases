# Changelog

Test builds of the Platebit Coach fork (branch `feature/jarvis`). Each version is published as a GitHub Release (`build-<versionCode>`) and arrives inside the app; the in-app "What's new" is in Greek (`release-notes/`).

## 1.19.3 (57)
- Updates now come from the public releases-only repo `jemangr/platebit-releases`; the code moves to a private repo. This build is published in both places, so phones on older builds move over with it.

## 1.19.2 (56)
- Fixed: Gemini Live could answer in Spanish. The voice language (Settings → AI Coach voice, Greek by default) is now sent as the speech language code, and the coach's rules open and close with "respond in Greek"; a model that refuses the language code is retried without it. The call reads the saved language when it starts.

## 1.19.1 (55)
- Fixed: Gemini Live could pick "gemini-3.8-live-extended-thinking" and stop at once with "Thinking level must be specified for this model". The plain Live model is now preferred (thinking, extended, preview and exp variants last), and a model that needs a thinking level is retried once with a low one, remembered for next time. Phones that had remembered the wrong model choose again.

## 1.19.0 (54)
- **Gemini Live** as a second voice for Live (switch at the top of the Live screen, shown when a Gemini key is set): microphone audio streams to Gemini's Live API over a WebSocket and its voice streams back; it answers while you talk and stops when interrupted. Live captions of both sides; mute, call again, end. The Live model is found from the key's own model list (remembered, looked up again if it disappears). Call-style audio on the speaker or a headset with the phone's echo canceller; phones without one pause the microphone while the coach speaks. Same coach rules and user data as the chat, written for speech; it can't change data during a call, and the transcript is added to the chat afterwards. Counts towards AI usage and respects "stop at the limit". Clear messages for a refused key, used-up free quota and the session time limit.

## 1.18.1 (53)
- **Medications in the diary by part of the day**: one row for "Morning medicines" (midday, afternoon, evening, night) with one Took them / Forgot them for all of them, instead of a row per medicine. A row shows only while it's due (from 30 minutes before) and disappears once answered, with Undo; otherwise the card says what's next, or that the day is done. As-needed medicines moved behind an "As needed" button. Every medicine is listed in detail only in the notification, whose buttons say Took them / Forgot them when there are several. The widget follows the same wording.

## 1.18.0 (52)
- **Weekly review** every Sunday at 20:00 (own switch in Reminders): average calories vs target, days on target, days logged, protein, water, trend weight change, workouts logged vs planned, medicine doses taken, and one tip for next week (logging, protein, calories, water or training, in that order). Opens History. Skipped for a week with nothing logged.
- **Automatic backup** (Settings → Data): weekly to a folder picked with Android's folder picker, the last 8 kept, Back up now, change folder, turn off; tells you when the folder can no longer be written.
- **Backups are now complete** (version 3): medicines and their doses, logged workouts, meal plans and the training program are included; older backup files still import and leave those as they are. Erasing all data now also clears them.

## 1.17.0 (51)
- **Workout logging**: Start on any workout of the program (suggested or your own). Sets come pre-filled with last time's weight and reps (matched by name, so Hevy's "Bench Press (Barbell)" counts as Bench press); when every set reached the top of the range, the next weight is suggested (+2.5 kg upper body, +5 kg lower body) with the reps back to the bottom. Tick a set to start the rest timer (+30 s, Skip). Seconds instead of reps for planks and other holds. The session is saved on every change and survives the app being closed; only ticked sets are logged. Training shows the session in progress, the last three workouts (minutes, sets, volume) and the count for the last 7 days.
- **Hevy CSV import** (Training → Import from Hevy): one row per set grouped into workouts; warm-ups left out; pounds converted; already-imported workouts skipped.
- The AI Coach gets the last 14 days of logged workouts with every set.

## 1.16.0 (50)
- **Home-screen widget** "Platebit · Today": calories and protein left (or over) with a progress bar, water with a +250 ml button that logs straight from the home screen, and today's medicines (taken so far, the next or overdue dose). Tap to open the diary. It redraws when meals, water, doses or targets change, and every 30 minutes for a new day. Name and description in Greek too.

## 1.15.0 (49)
- **Shopping list** from the meal plan (cart button at the top of the plan): from today or the whole week, grouped by aisle; foods add up across days; cooked rice, pasta, legumes, meat and fish are converted to raw or dry amounts; eggs, pitas, cans and bars are counted; liquids in ml. Approved days are used when there are any. Ticks are kept per week; Share sends the list as text.
- CI shows Gradle's error as annotations when the tests fail.

## 1.14.0 (48)
- **Health Connect guide** ("How to connect correctly"): Platebit only reads; one app per kind of data; why weight doubles are harmless but food and workout doubles inflate calories; setting App priority in Health Connect; which apps write each kind of data on this phone, with a warning when two write workouts. A short version is shown before connecting.
- Workouts written by two apps for the same session (watch + Hevy) are merged and counted once.
- Greek layout: exercise sets/rest under the name, history's days-logged under the date, shorter Greek labels.
- **New features explained**: after an update, one detailed notification a day at 19:00 per important feature, opening it on tap; its own switch in Reminders.

## 1.13.1 (47)
- Fixed: the meal plan could stay empty (0 kcal) on first open, because suggestions were made before the targets loaded; empty days now refill.
- Diary shortcuts no longer cut their labels; "left" is translated.
- Screen tests (Robolectric + Roborazzi) render the main screens in English and Greek on every build; README screenshots come from them. WorkManager now starts on demand.

## 1.13.0 (46)
- **Describe food in words** from the diary's +: "6 boiled eggs" or "1 pork gyros souvlaki, no-oil pita, tomato, yogurt"; the AI splits it into foods with grams to check before logging.
- **Medication reminders**: per medicine a label (kind and colour), dose, frequency (every day, some days, every other day, as needed) and times; its own reminder channel with Took it / Forgot / In 15 min; a Medications card in the day's log; asked per medicine whether the AI Coach may see it (private unless the user says yes); exact times with Android's alarms permission. Offered in the setup's health step.

## 1.12.3 (45)
- Fixed: changing the language didn't stick. The choice is now saved before the app restarts.
- Fixed: rows with a long value (meal times) squeezed their title into one letter per line; long values now sit under the title.
- The app appears in Android's per-app language settings (Settings → Apps → Platebit → Language).
- README rewritten for everything below; this changelog added.

## 1.12.2 (44)
- Updates: when the phone cancels an install started by the app (Xiaomi and others, even after Update is tapped), Platebit says so and offers **Android's own installer** with the file already downloaded. Failures stay on screen instead of quietly showing "ready" again.

## 1.12.1 (43)
- In-app "What's new" now covers every version since the last release; CI runs queue instead of cancelling, so every version gets its release.
- The download link always points to the newest build (`releases/latest/download/Platebit.apk`).
- Lint: explicit notification permission checks.

## 1.12.0 (42)
- **Reports and export**: a week, month or 3-month report (averages against targets, days on target, water, trend weight, training and steps), saved as PDF. Daily totals as CSV (free: last 30 days; Pro: everything), all meals and foods as CSV (Pro), full JSON backup always free. Pro is open to everyone while testing.

## 1.11.0 (41)
- **Training program**: suggested from the setup answers (full body, upper/lower or push/pull/legs; gym or home; sets, reps and rest by goal and level; when to add weight), or the user's own, typed one exercise per line, with a responsibility notice. The AI Coach sees the program in use.

## 1.10.0-ai2 (40)
- **AI use and limits**: requests, scans, coach answers and tokens per day, week and month; daily and weekly limits with notifications at 80% and 100% and an optional stop.

## 1.10.0-ai1 (39)
- **AI setup wizard**: plain comparison of the services, OpenRouter in one tap (OAuth PKCE), three steps and a key-page button for Gemini, DeepSeek, OpenAI and Claude, key pick-up from the clipboard, test call and activation. Offered when scanning without an AI.

## 1.9.0 (38)
- **Health Connect** (read only): steps, active calories, workouts, sleep and heart rate, and weigh-ins from other scales; an Activity card on the diary; the coach sees the last 14 days.

## 1.8.0-history1 (37)
- **History** of days, weeks and months.
- **Own values** instead of the calculated ones: calories, water, macros and meal times, with a disclaimer.

## 1.8.0-plan1 (36)
- **Meal plan**: a week of meals from calories and protein with Greek foods, no AI; lock, swap, own meal, ate it, change amounts, approve the week; portable options when away.
- **Reminders, each with its own switch and channel**: meals ("did you follow the plan?"), water (only when behind, stops before sleep), protein, training days, weigh-ins, new versions.

## 1.7.0 (35)
- **Greek** throughout the app, with a language picker; the coach and meal names follow the language.

## 1.6.0 (34)
- **Packaged food**: package and serving sizes from the AI and Open Food Facts; quick amounts (servings, ¼, ½, whole package, 100 g); label values per 100 g, per package or per serving.

## 1.5.0-coach4 (33)
- **In-app updates** from GitHub Releases, a notification for each new version with what's new, and "What's new" after updating.

## 1.5.0-coach3 (32)
- **Onboarding v2 and plan engine**: goal, training history and routine, workout tracking, day schedule, supplements and health notes; calories (Katch-McArdle with body fat), protein by goal, carb cycling, water with add-ons and a doctor's cap, last-drink time, meal times, training split and safety notes.

## 1.5.0-coach2 (31)
- Coach renamed **AI Coach**; **Live** voice conversation (listen, answer aloud, listen again; "yes"/"no" answers proposals).

## 1.5.0-jarvis1 (30)
- Coach button in the middle of every main screen; talk and read-aloud; the coach only discusses app topics; GitHub Actions build.

## 1.4.0
- The original Platebit by Joji228.

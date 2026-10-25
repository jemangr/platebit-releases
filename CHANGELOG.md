# Changelog

Test builds of the Platebit Coach fork (branch `feature/jarvis`). Each version is published as a GitHub Release (`build-<versionCode>`) and arrives inside the app; the in-app "What's new" is in Greek (`release-notes/`).

## 1.25.0 (66)
- **Routine library** (Training → Routines): save any number of routines outside the program, put any of them on any weekday with the day chips, start one or an empty workout. The week plan, the Today card, the home training card, the training reminder and the coach all see them; switching program keeps the routines' days. Names stay unique.
- **Save as a routine** on a finished workout's stats screen: its exercises, working sets, reps, rest, warm-ups and supersets.

## 1.24.1 (65)
- **Report a problem** (Me → Help): the app version, phone, the last crashes (kept on the phone) and the app's own recent warnings and errors as a text file to share; no meals, weights, health data, medicines, keys or chat, and key-like strings are masked.
- **End-to-end tests on an emulator** in CI on every push: setup with the consent, every tab and the meal plan, with screenshots, logcat and crashes kept with the run.

## 1.24.0 (64)
- **Meals you always have**: "Always this" on a planned meal keeps it in its slot on the chosen weekdays (coffee every morning, a koulouri on school days, yogurt with honey at night). Every new plan keeps it and the other meals share what's left of the day's calories and protein, each between half and twice its usual size.
- **Meals you don't have**: "Meals of the day" (the sliders icon) turns a slot off for all or some weekdays, e.g. no afternoon snack; its share goes to the others.
- **The day closes on target**: whenever a meal is logged, from the plan or not, today's meals still to come are resized to what's left (marked "Resized to what you ate earlier today"). "Another option" keeps the meal's size.
- **Weight forecast** (Progress): the weight in 1, 3, 6 and 12 months if the plan is followed exactly, day by day from the calorie target and the burn at each weight, stopping at the goal, with a likely range; and how the trend compares with what the plan expected since the start. Recomputed at every weigh-in; the coach knows it too, and the meal-plan rules.
- **Your own meal with AI**: the AI fills in the calories and macros from the meal's name.
- **Onboarding above the keyboard**: the page ends above the keyboard, so the field being typed in and the Continue button stay reachable.
- Meal-plan rules are in the full backup.

## 1.23.0 (63)
- **Five tabs**: Today, Plan, AI Coach (middle), Progress, Me. Plan gathers today's meals and workout with the meal plan, shopping list, program and exercise library; Progress the weight trend, the training week, recent workouts, history, measurements and reports; Me is Settings with the new logo.
- **The Now card** on Today: the one next thing with one button, most urgent first: a workout in progress, medicines due, a meal that's due and not logged (with "Ate it as planned" when there's a plan, or "Didn't eat"), today's workout around the gym time, the morning weigh-in, water when 400 ml or more behind; otherwise "All done for now" and the next meal.
- **The home screen follows how you train**: lifting (weights, CrossFit) puts a training card with today's workout and the week's volume against the same days last week right after calories; endurance (running, cycling, swimming, sports, classes) puts activity first and shows active minutes; walking puts steps first. Medicines move up when you take any, weight when you weigh in 5+ days in 2 weeks, water goes last if you never log it.
- **Disclaimer and health-data consent** (GDPR art. 9): a step in setup and a one-time screen for existing users, with the full text in Me → Legal.
- **New logo**: four scanner corners on the icon, the wordmark on the welcome screen, Me and workout share cards.
- **Sharing a workout is free** (always with the logo); during the free beta every Pro feature is on.

## 1.22.2 (62)
- **Meal scans never think**: DeepSeek scans send thinking disabled, Gemini scans ask for no thinking (2.x) or "minimal" (newer), falling back to "low" and then the default if a model refuses. The DeepSeek thinking setting now applies to the AI Coach only.

## 1.22.1 (61)
- **Gemini Live can log and change things** like the chat: it calls functions that put the same Accept cards on the call screen (and in the chat), and a spoken yes or no answers them. Before, the call's rules said it couldn't change anything and sent you to the plus button. A Live model that refuses functions falls back to talking only.
- **Meal scans can't hang for minutes**: each engine gets 60 seconds, then the backup engine (if set) or an error. After 8 seconds the progress screen shows which engine and model it's waiting on and for how long.

## 1.22.0 (60)
- **The AI Coach can change almost everything**, always as a card you accept: a meal's grams, macro and step targets, a body measurement, wake, sleep and training times, reminders on or off, a meal in the meal plan, and the training program (sets, reps, rest and warm-ups; adding, removing or swapping an exercise; which days each workout is on; the weight for next time, filled in when you start). Medicines, settings, backups and AI keys stay yours to change. A program change to the suggested program makes it your own copy.
- **A workout for each day of the week** (Training): pick the days under each workout. The Today card shows today's workout with Start (or a rest day), and the training reminders follow those days and name the workout.
- **Readiness before a workout**: push, hold or go easy, from last night's sleep, resting heart rate and HRV against your own week, yesterday's calories and protein, and how soon the same workout comes again. "Go easy" makes the remaining sets 10% lighter with one less, in one tap. HRV is now read from Health Connect.
- **Steps without Health Connect**: the phone's own step counter, read every 15 minutes (Android's "Physical activity" permission). Health Connect's steps win when there are both. Tap steps for the day against the goal and the last 14 days; tap a watch workout for its details.
- **Find it with AI**: when Open Food Facts doesn't have a food, one tap sends what you typed to the AI, no retyping (adding a meal and adding a food to a meal).
- **Faster Gemini**: chat and scans ask Gemini to think little (it thought for 20–40 seconds by default); the coach's data now ends with the time, so DeepSeek and Gemini can reuse the rest from their cache (cheaper and quicker). **Gemini Live** answers about half a second after you stop and notices interruptions sooner; its call uses a smaller part of your data.
- **Updates no longer stall**: a download that stops is resumed from where it stopped (up to 6 times), and a failed update offers the browser instead.

## 1.21.0 (59)
- **Exercise library with photos** (free-exercise-db, public domain): 870+ exercises with two photos each (start and end position), main and helper muscles, equipment, level and step-by-step instructions. Photos load when an exercise is shown and are kept on the phone, so the app doesn't grow. Search by name, filter by muscle group and equipment, recent ones first, tap ⓘ for how to do it. **Create your own** exercise (name, main muscle, equipment) when one is missing. The picker opens from Add exercise in the routine editor (several at once) and in a workout in progress, where an exercise can also be replaced or removed. Thumbnails in routines, workouts and stats. Names stay in English.
- **Stats after every workout**, full screen, and from any logged workout (Training, the diary's Activity card): minutes, working sets, reps and volume against the last time you did the same workout, the records it beat (one line per exercise), sets per muscle group (helpers count half), each exercise with its best set and volume change, and the week so far: workouts against your training days, sets, kilos and weeks in a row with a workout.
- **Share this workout** (Pro): a new picture with the four numbers, muscles, best set of each exercise and records, the app's name and its link; the message under it (the caption on WhatsApp and Viber) repeats the numbers with the link.
- **Export workouts (CSV)** in Hevy's columns, so they open in a spreadsheet and go back into Platebit, Hevy or Strong.
- **Body measurements** (Training → Body measurements): waist, hips, chest, neck, shoulders, arm, forearm, thigh and calf, one per part and day, with a line over time and the change since the first. In the backup, and the AI Coach sees them.
- **Activity card**: the day's workouts, logged in Platebit (tap for the stats) and from Health Connect (the same session from the watch counts once); steps against a daily goal with a progress line. Tap **sleep** for bed and wake time, the stages (deep, light, REM, awake) and the last 7 nights; tap **heart rate** for lowest, highest, average, resting and the last reading with its time, and the last 7 days. Without any activity data the card offers to connect Health Connect.
- **Daily step goal for everyone** (Settings → Goals → Daily steps): a suggestion you can change; with two weeks of steps, about 10% above what you already walk (5,000–12,000), otherwise 8,000 (7,000 from 60, 10,000 for walkers). "Walking" is now a training type in setup.
- **History** now has steps, sleep, active calories and workouts per day, and goal chips that turn green or orange: protein, water and steps per day, and how many days of the week or month each goal was reached.
- **Medicines calendar** (Medications): a month with every day green (all taken), orange (some missed) or red (none taken); tap a day to see what wasn't taken and what was. Today is coloured once everything due is taken.
- **Water**: an arrow next to +500 ml opens 150 ml to 1.5 L; the amount you pick is logged and becomes the button's default.
- **Sounds and watch** (Settings → Reminders): a sound for each kind of notification from Android's list, alarm tones included, and phone and watch or phone only. "Watch only" comes with the watch app (phase C).
- Backups include your own exercises and body measurements.

## 1.20.0 (58)
- **Routines like a gym app** (Training → My own → Write or Edit): exercises in order with sets, reps, rest (1:00 to 3:00 or none) and warm-up sets; link an exercise to the next for a superset; move up or down; start from the suggested program; paste lines as text as before.
- **Set types while logging:** tap a set's number for warm-up (W), drop set (D), to failure (F), RPE 6–10, or remove it. Warm-ups are pre-filled at half the working weight and never count for records, volume or the next weight. Hevy imports keep their set types and RPE.
- **Supersets while logging:** no rest between the exercises of a round, the rest after its last; the card says which exercise is next.
- **Records:** a set that beats your heaviest weight or best estimated 1RM is marked PR. **Exercise history** (tap an exercise, or Training → Exercises and records): heaviest weight, best estimated 1RM (Epley, 1–12 reps), best session volume, a chart of the estimated 1RM per session, every session.
- **Rest timer with the screen off:** while a workout runs, an ongoing notification shows its time and the rest counting down; at the end of a rest the phone vibrates with a short alert. Tapping it opens the workout.
- **After Finish:** a summary with minutes, sets, volume and the records you beat, and a picture of the workout to share (Pro).

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

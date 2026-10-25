# Changelog

Test builds of the Platebit Coach fork (branch `feature/jarvis`). Each version is published as a GitHub Release (`build-<versionCode>`) and arrives inside the app; the in-app "What's new" is in Greek (`release-notes/`).

## 1.44.0 (95)
- **Music on walks and workouts** (`music/Music`): play/pause, next and previous through the media session of the chosen app (with notification access, `MusicListener` only for MediaSessionManager) or Android's media keys to whatever plays; the song's title and artist; Settings → Devices → Music: app, play when a walk or workout starts. A music bar on the walk and workout screens.
- The watch gets the song (`WatchMusic`) and ⏮ ⏯ ⏭ on the walk and workout screens (`MUSIC_*` messages; icons drawn in the app).

## 1.43.2 (94)
- **Routines from Hevy's API** (`HevyApi`, `HevyRoutines`): with the user's Hevy Pro key (pasted in a dialog, used once, never saved) every saved routine comes in with its folder, rep ranges, rest, notes, warm-ups and supersets (`/v1/routine_folders`, `/v1/routines`, 10 a page, `api-key` header); they replace the routines guessed from the CSV history with the same name.

## 1.43.1 (93)
- Hevy import: US-style dates ("Sep 20, 2026, 4:59 PM", narrow spaces before AM/PM) were not read, so newer exports brought nothing; more date layouts, case-insensitive, and a clear error when no date in a file can be read.
- Routines from an import come from the fullest of a title's last three sessions.
- Band exercises with kilos logged stay weight and reps.

## 1.43.0 (92)
- **Walking Live** (`walk/`): a health|location foreground service with the step counter, GPS outdoors (haversine between fixes within 25 m accuracy, no jitter under 2 m, no jumps over 15 km/h; steps × stride from height while GPS has no fix or on a treadmill), pace, speed and cadence over the last minute, a voice every km, pause and resume, Japanese interval walking (3′ fast / 3′ easy × 5, buzz and voice at each change). The route isn't kept; the walk is saved as a distance-and-time workout with the steps in its note. A walk button bottom left on Today, opposite the camera, showing the steps while walking.
- The watch shows a walk (`WatchWalk`: steps, time counted on from the phone's last word, km, pace, cadence, interval phase) with Pause/Resume and Finish (`WALK_*` messages).
- **The watch app opens by itself** when a workout or a walk starts on the phone (`WatchLaunch`, RemoteActivityHelper with a `platebit://wear` link the watch answers); Settings → Devices → Open on the watch.
- **Lean mass** from Health Connect scales (`LeanBodyMassRecord`, a new read permission, asked from Settings by people already connected), shown with body fat on the weight card.

## 1.42.0 (91)
- **Free food analyses without a key**: an install with no AI set up gets 10 a month through Platebit Pro AI Cloud after an OK on the scan screen (photo through api.platebit.app to DeepSeek, not kept). The app makes a random install key (`pf-` + 32 hex, not in backups); the worker keeps only its SHA-256 (`free_users`), counts use under the negative id, allows scans only, `free_month_scans` / `free_day_requests` / `free_total_month_usd` / `free_day_new` (new installs per connection a day, salted daily hash) in the admin page. Used up → an offer of a Pro AI Cloud place or an own key.
- **Streaks and milestones** (`Achievements`): days in a row with food logged, weeks in a row with a workout, 20 milestones (logging, training, nutrition, body), on the Progress tab and a new screen.
- **Month and year recap** with a share card (`ShareCards.renderRecap`): days logged, workouts, volume, average kcal and protein, trend weight, best streak, most-done exercise.
- **Week in review** on Today on Sunday (the week so far) and Monday (the week before): wins and misses from the numbers, "Ask the coach what to change" opens the chat with that question.
- **Ordering in or eating out**: the coach (chat and Live) suggests 2-3 dishes for that kind of place against what's left of the day; scans and the coach expect other cuisines too; 60 taverna and delivery dishes in the offline food list.
- Resistance-band exercises default to reps only; the kind texts mention cables and bands.
- Watch screenshots (Robolectric + Roborazzi for the Wear app and the food tile via the tiles renderer) and the GitHub social preview.

## 1.41.0 (90)
- **How each exercise is logged** (`TrackKind`: weight and reps, reps only, assisted, weighted bodyweight, time, distance and time), guessed from the name (whole-word cardio match, library category) and kept per exercise (`WorkoutStore.kinds`, in backups as `exerciseKinds`). Cardio sets keep `LoggedSet.km` with the seconds in `reps`; pace and km/h shown; records for longest distance, longest time and best pace; volume, 1RM and weight records only for lifted kinds; Hevy import/export carries distance.
- One tap applies the suggested next weight to the sets left.
- **Strength level** (`StrengthLevels`) for squat, bench, deadlift and overhead press from the estimated 1RM against body weight and sex, with the kilos for the next level.
- **Strong CSV import** (`StrongCsv`: old and new layouts, `,` or `;`, lbs, metres and miles, warm-up/drop/failure sets, durations).
- Weight card: body fat from Health Connect scales too, and how long since the last weigh-in; recent weigh-ins with body fat move the card up (`HomeUsage.scale`).
- Setup ends with "Scan your first meal", opening the camera (or "Later, go to Today").

## 1.40.1 (89)
- Platebit Pro AI Cloud shows no money: the balance card becomes "Left this month" with food analyses and coach messages left (`ApiBalance.allowances`, no amounts, so no low-credit alert either).

## 1.40.0 (88)
- **Platebit Pro AI Cloud** (`cloud/`, Cloudflare Worker on api.platebit.app, D1): the app's OpenAI-format requests go through to DeepSeek (`deepseek-flash`, thinking capped, max 4096 out); a tester's code (`pb-…`, only its SHA-256 stored) is the key; per tester 150 food analyses (a photo, or `X-Platebit-Kind: scan`) and 300 coach messages a month, 60 requests a day and $1.50 a month as caps, a monthly total, an off switch; usage counters only, no bodies or headers logged. Admin page at `/admin` (ADMIN_TOKEN): testers, codes, use, limits, prices. Deployed by `.github/workflows/cloud.yml` (node tests, then `wrangler deploy`).
- App: new engine `Provider.CLOUD` (before Custom): model picked by the server, Cloud refusals in the user's language, what's left this month as the balance (`/v1/me`). Setup offers the free beta places (`/v1/signup`): a name and one tap make the code, save it and make it the engine; one sign-up per connection a day, places set in the admin page.
- **Google Drive backup** (`DriveBackup`, `drive.file` scope via the Identity AuthorizationClient, Drive v3 over OkHttp): weekly on Wi-Fi, the same full backup as the folder one in a visible "Platebit backups" folder, the last 8 kept, same-day replaced; Settings → Data. Setup's welcome screen: Restore from Google Drive, picking one of the backups.
- **Wear OS food tile and complications**: the phone sends today's totals and targets (`WatchFood`, on every meal and plan change); the watch shows a calories ring with protein, carbs and fat (`FoodTile`, ProtoLayout material) and four complications (ranged value / short text), refreshed when the totals change and every 30 minutes, 0 on a new day.

## 1.39.0 (87)
- Wear OS: **rest by heart rate** (opt-in, Devices → Platebit on Wear OS): the ongoing workout keeps the heart-rate sensor on; past 45″ and 3 s at or below the target (halfway between resting and Tanaka maximum) the rest ends with a buzz (`REST_SKIP`), still high at the planned end it goes on 15″ at a time (`REST_MORE`), up to 90″ extra.
- Wear OS: **rep counting (beta)** (`RepCounter`): per-axis high-pass and smoothing of the accelerometer, one count per full swing with hysteresis, the most-moving axis wins; it fills the reps, the user checks them.
- The watch's settings live on the phone (`WearSettings`) and travel with the state.
- **Short setup** by default: Get started asks only basics, weight, goal, health and a new devices step (Health Connect connect button, Wear OS status), then the plan; "Or answer everything now" on Welcome keeps the full flow (with the devices step after tracking). Training, day, food and supplements can be filled later from Settings → Profile and goal or by the coach.

## 1.38.0 (86)
- **Wear OS app** (new `:wear` module, same package `app.morsel` and key; Compose for Wear OS): the phone's workout in progress with the next set, − / + for weight and reps, ✓, rest countdown with Skip and +30″ and a buzz at the end (an ongoing activity keeps it going with the screen off), live heart rate from the watch, Start for today's workout, Finish with two taps, a notification when a workout starts on the phone.
- Phone ↔ watch over the Data Layer (`WatchProtocol`): the state as one urgent data item, sent only when a watch declares the `platebit_wear` capability; buttons as messages to `WearActionsService`, which ticks the set with the watch's numbers (`LiveWorkout.withNumbers`). With a watch paired the phone's rest-over alert stays on the phone.
- CI: the watch APK goes to its own `watch-v<code>` release (never "latest"); the updater picks the phone APK by name. e2e counts only Platebit's own crashes (the emulator's Gmail crashed the last runs).

## 1.37.0 (85)
- **Training widget** (`TrainingWidget`, RemoteViews with chronometers): during a workout the next set, elapsed time, rest countdown, ✓ Set (ticks the next set with its pre-filled numbers via `LiveWorkout.tickNext` and starts the rest), Skip, +30″; otherwise today's workout with Start (starts it through `TodayWorkout.start` and opens it), or the rest day and next workout, the week and the streak. Redrawn when the session, rest, log or program change.
- **Workout notification** shows the next set and has the same buttons.
- **Workout photos** (`WorkoutLog.photos`, `WorkoutPhotos`, max 6, 1600 px JPEG, never in backups): camera or gallery, on the live workout and its summary; removed ones are deleted once saved, discarded sessions take theirs; erase-all clears them. Share card option "Photo": the clear card over the photo, as JPEG.

## 1.36.2 (84)
- Routine links: the `https://platebit.app/r` intent filter has `autoVerify`, so once `/.well-known/assetlinks.json` is on platebit.app they open Platebit directly.

## 1.36.1 (83)
- Quick add keeps the text: switching from Describe to search or manual carries it into the search and the name. A "From the food list, no AI" row reads typed amounts at once ("3 eggs") and opens them on the meal screen.
- **Your products** (`ProductStore`, by barcode, in the backup): every barcode hit on Open Food Facts is kept (next scan instant and offline); an unknown barcode plus a photo of its label is learned from the AI reading (only for one packaged item: barcode in the photo, or a brand / pack / serving size seen), never overwritten by a lookup. The viewfinder reads barcodes with Open Food Facts off too; saved products show first in search.

## 1.36.0 (82)
- **Health Connect reads more**: distance (next to steps), SpO2 (heart sheet), body fat (attached to imported scale weigh-ins), water from other apps (imported once per record id, switchable); all in the coach's activity table. Platebit's own workouts are filtered out of the read.
- **Write to Health Connect** (opt-in, its own permission request): meals as `NutritionRecord` (energy and macros, meal type), water as `HydrationRecord`, Platebit workouts as `ExerciseSessionRecord`, last 7 days, with stable client record ids (`platebit-meal-12`…), a version bumped only when content changes, deletes mirrored; imported water is never written back. Turning it off offers to delete what was written.
- Privacy text, connection guide and legal terms say what is written.

## 1.35.1 (81)
- **Built-in food list** (`FoodList`, ~90 common Greek foods with per-100 g values and piece/slice/portion weights) and a phrase reader for Greek and English ("3 βραστά αυγά", "2 φέτες ψωμί και 200γρ κοτόπουλο", "καφές με γάλα"): amounts, units, sides, meal names.
- Used without internet: the coach answers an offline food message with a Log card from the list (source "Platebit food list"), Describe in Quick add falls back to it, and search shows list foods first, then Open Food Facts. Quick add is offered to everyone.

## 1.35.0 (80)
- **Meal times by kind of day** (`MealDays`): weekdays, weekends (`DaySchedule.weekend`: own wake/sleep, no work unless `worksWeekends`) and training days (`gymDay` times; the meals before and after the workout are marked); one day on its own (`DayPlan.times` / `skip`, planned again with `MealPlanner.refit`, dishes kept). Meal reminders, the plan sync and the coach's data follow each day's times.
- **Suggested times with Yes / Change times / No** on the meal plan, plus the user's day in their own words turned into times by the coach; new coach action `set_meal_times` (scope today / weekdays / weekends / training days / every day; `skip` for today only), in chat and Live.
- **2 more options per meal**, same size, one tap (`MealPlanner.options`); **favorites** from locked meals come up about every other day (`MealPrefs.favorites`); **cook once for two days** (`cookFor`, `leftover`, `withLeftovers`, kept consistent on every store write by `linkLeftovers`), per dinner or for every dinner (`MealPrefs.leftovers`).
- **Offline AI errors** (`NetworkTrouble`): "no internet" instead of hosts and ports; the coach sends the waiting question by itself once the phone is back online (`Online.whenBack`); connect timeout 12 s.

## 1.34.0 (79)
- **Coach persona** (`CoachingProfile.coach`): name (default Arnie / Άρνι), gender (male by default; Gemini Live voice Orus or Kore, retried without a voice when a model refuses; the phone's speech engine picks a voice of that gender when it labels them), manner (strict, balanced, friendly) and check-ins; the persona leads the coach's data in chat and Live, and the titles show the name.
- **Evening check-in** (`ReminderKind.CHECK_IN`, 2½ h before sleep, never before 19:30): the day's calories, protein and water against the targets and the one thing still to do, worded by the manner; the Sunday review follows the check-in choice.
- **AI credit alert**: a threshold ($1–$10) checked at most every 3 hours after AI use for DeepSeek and OpenRouter, once a day; every AI warning now says what to do (backup engine, top up, AI Cloud).

## 1.33.0 (78)
- **Diet preferences** (`CoachingProfile.diet`, a new setup step): styles (Mediterranean, vegetarian, vegan, pescatarian, keto, Orthodox fasting on Wednesdays and Fridays, intermittent fasting with an eating window), meals a day (3/4/5), allergies and intolerances (never relaxed, in every fallback), dislikes, cooking time, budget, eating out and alcohol a week; all told to the coach.
- **The meal planner follows them**: foods carry tags (meat, fish, shellfish, milk, lactose, egg, gluten, nuts, peanut, soy, sesame, honey), suggestions carry cooking minutes, cost and keto; 17 new suggestions (plant-based, fasting, keto, gluten-free breakfasts); soft wishes give way before the diet, the diet before nothing, allergies never. Keto suggestions only for keto. Daily whey goes into a snack.
- Meal slots follow meals a day and the eating window.

## 1.32.0 (77)
- **Lifestyle** (`CoachingProfile.life`): job (sitting ×1.0, standing ×1.05, manual ×1.12 on the burn, skipped when the smart target measured it), stress and shifts, asked in setup and told to the coach.
- **Supplements**: casein; creatine's start date (chips) with an optional daily reminder created under Medicines; the last coffee's time, flagged when under 6 h before bed (`SupplementNotes`), for the coach's sleep advice.
- **Notes by the weight forecast** (`GoalNotes`): what building muscle (+5/7/10% by level) or recomposition (up to −10%) means for the scale, and creatine's water weight during its first 4 weeks.

## 1.31.0 (76)
- **Share cards in three looks and two sizes** (`CardLook`): dark, light (deeper green accent, dark wordmark) or clear (transparent PNG, translucent tiles, shadowed text, for stickers over a photo), as a 4:5 post or a 9:16 story; a preview dialog before every picture share (day, progress, workout), remembering the last choice.

## 1.30.0 (75)
- **Routine folders**: `Workout.folder`; Routines groups them under foldable headers (rename, or empty the name to un-folder); ⋮ per routine: Share, Move to folder, Duplicate, Delete (its week-plan days go too).
- **Routines by link and QR, no server**: `RoutineShare` packs the routine (deflate + base64url) after `https://platebit.app/r#`, so it never reaches a web server; the share sheet sends the message with a QR picture, and the dialog shows the QR to scan. Import from a pasted message (the clipboard is read when it holds one) or a picture of the QR (ZXing); names and numbers are bounded. `platebit://r` and `https://platebit.app/r` open the app with the import prompt (the https one opens directly once the site serves assetlinks.json).
- e2e: a picture right after tapping Me.

## 1.29.1 (74)
- **Live animation**: an aurora orb (three shifting colour blobs, a bright heart), a ring of 72 sound bars that follows the user's voice while listening and the coach's own voice (RMS of the played audio) while speaking, orbiting specks, and two comets while thinking; still frames for screenshots (`frozenAt`).
- **The coach's exercise changes reach the workout in progress**: replace, add and remove apply to the running session of that workout too (never to exercises with done sets).
- Meal types follow the language in use (they were fixed at first load); long meal names wrap to two lines.
- New screenshots: 36-scan-result and 37-live-coach (for the site's demo), 35-live-orb.

## 1.29.0 (73)
- **Hevy parity, phase 2 (progress)**: per-exercise charts with a metric (estimated 1RM, heaviest weight, best set volume, session volume, total reps/seconds) and a range (1 month, 3 months, 1 year, all); set records (heaviest weight per rep count, only real records); a 1RM calculator with a 100–50% table and Epley reps; a training calendar with tappable days and a weeks-in-a-row streak; sets per muscle group over 7/30/91 days against the period before. Both are linked from Progress → More.
- The meal plan header keeps its title on one line (the week arrows sit next to the week).
- e2e: the meal-plan step goes back with the screen's Back button.

## 1.28.0 (72)
- **Hevy parity, phase 1 (in the gym)**: a "Previous" column per set (last session's set in the same place, warm-ups matched to warm-ups; tap to copy); plate calculator (bar 20/15/10/none, the gym's plates, a drawing of one side, the closest loadable weight); warm-up calculator (2/3/4 steps from the first working set, rounded to 2.5 kg, never under the bar); notes per exercise for the session or pinned to the exercise, and a workout note; move exercises up/down mid-workout; a record toast and haptic the moment a set beats your best; a countdown for timed sets that ticks the set and starts the rest; rest −15″/+15″; correcting a saved workout (sets and notes through the logger, name/day/start/length, delete).
- Hevy CSV import and export carry workout descriptions, exercise notes and superset ids. Pinned notes and the plate setup go into the backup.

## 1.27.0 (71)
- **Logo bar on the tabs**: Today, Plan, Progress and Me share a fixed bar (stays while scrolling): share on the left, the Platebit wordmark centred, then report a problem (beta builds only), notifications and settings. The bell and gear left the page headers; the camera keeps its full screen.
- **Share from each tab** (`ShareCards`): a day card picture (calorie ring, macros, meals, water, steps, workout), today's meal plan and workout as text, a progress card picture (trend line, goal, 3-month forecast) and an invitation. No medicines or health details on any of them.
- **Shake to report a problem**: three strong jolts within 1.5 s on any screen ask to send the problem report; on by default, switch in Me → About.

## 1.26.3 (70)
- **Restore on the first screen**: after a reinstall or on a new phone, "Restore from a backup" on the welcome page reads the backup file (any folder, Drive, Downloads) and goes straight to Today when it carries a profile; the consent gate then asks once. Android erases the app's folder permission on uninstall, so the file is picked, not found by itself.
- **Hevy during setup**: choosing Hevy as the way you record workouts shows an import card with the export path; the CSV brings history, records and routines (shared `HevyImport` with the Training screen).

## 1.26.2 (69)
- **Save to Google Drive** (Me → Data): the full backup handed to the Drive app's "Upload to Drive", where the user picks the account and folder; shown when the Drive app is installed. Automatic Drive backups (Drive API, drive.file, needs a Google Cloud OAuth client) are planned for later.

## 1.26.1 (68)
- **Routines from Hevy**: Hevy's CSV holds the history, not the routines, but every workout carries its routine's name, so importing now also saves one routine per title from its latest session (working sets, reps, warm-ups) next to the history, records and last weights it already brought. Names the user already has are left alone.

## 1.26.0 (67)
- **Settings in seven groups**: Me shows your profile and AI at a glance, then Profile and goals, AI, Notifications, Devices, Data, Appearance and About, each on its own page (nothing removed, just grouped).
- **Bell and gear on the screens**: Today (notifications, profile and goals), Plan (notifications, profile), Progress (notifications, data), Meal plan (meal reminders), Training (notifications, training profile) and Medicines (their notification sounds). A red dot on the bell when Android has the app's notifications off. Connecting Health Connect from Today opens Devices.

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

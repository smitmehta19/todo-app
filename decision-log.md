# Decision log

## Current: the simple app (restarted 2026-09-23)

The project was restarted because the demos had grown too complex. Everything below the "Superseded" line describes the old demo phase and no longer applies.

| # | Decision | Type | Status | Reversible |
|---|----------|------|--------|------------|
| R1 | Restart with a minimal scope chosen by the user | Architectural | Done | Yes |
| R2 | One self-contained file, `app/index.html`, no build, no server | Architectural | Done | Yes |
| R3 | No accounts; data saved in the browser, with backup and restore files | Critical | Done | Yes |
| R4 | Groups with colours; tasks may be grouped or loose | Design | Done | Yes |
| R5 | Dark theme only | Design | Done | Yes |
| R6 | To use it on a phone, the file must be hosted at a web address | Critical | Open | n/a |
| R7 | Visual redesign: coloured group cards, big date, one characterful font | Design | Done | Yes |
| R8 | Old demos deleted (moved to the Recycle Bin) | Housekeeping | Done | Yes, from the Recycle Bin |
| R13 | Four pets to choose from (Mochi, Boo, Sprout, Miso the cat), playable by drag, flick, tap and tricks; empty-state blobs removed | Design | Done | Yes, "No pet" in Settings |
| R12 | Mochi the helper (tilt, cheers, peeks) and group bars that open in place on Today | Design | Done | Yes, Mochi can be switched off |
| R11 | Hosted on GitHub Pages; installable to the home screen; own git repo | Critical | Done | Yes |
| R10 | Background changed from dark cocoa to midnight blue | Design | Done | Yes |
| R9 | "Squishy" redesign through the impeccable process; Today first; real laptop layout | Design | Done | Yes |
| C1 | Git deferred; the home-directory repo must not be used | Critical | Done (R11) | n/a |

### R13. Pets (2026-09-24)
- Request: the owner found two blobs confusing (a green empty-state blob at the top that behaved differently from Mochi at the bottom). They asked for a choice of three pets with different personalities, Mochi being one, plus a fourth: a very cute cat that walks anywhere on the screen and does cat things. All pets should be playable: flick, move around, make them do actions. The pet is picked in Settings.
- Fix for the confusion: empty states no longer draw a blob. Only the chosen pet is ever on screen.
- Engine: one shared physics loop (gravity, wall bounces, floor = add box top, runs only while something moves), pointer drag with velocity sampling for flicks, tap, press-and-hold menu, and a per-pet definition (art, weight, bounce, tricks, voice). Miso adds a small behaviour loop and climbs task cards, found again by selector after each redraw and scroll.
- Settings: "Your pet" row with a Change button opening a picker of four cards and "No pet". Tilt switch kept for phones.
- Data: `mochi` settings became `pet` (kind, tilt, greeted), migrated from older saves and backups.
- Rejected: several pets at once (clutter, and the original complaint was two creatures); sounds; pets reacting to deleting.
- Verified in the browser: every pet spawns and speaks; every trick runs without errors; flicking Mochi (bounces, lands, speaks); flicking Miso (tumbles, lands on its feet, grooms); Miso walking, hopping onto the add box, jumping onto a task card, riding it while scrolling and dropping off when it leaves; tap purr and hearts; Sprout growing leaves and blooming when today is clear; Boo gliding in; Nap with Undo; migration of old saves; phone and laptop layouts.
- Not verified: real touch flicks, gyroscope and shake on a physical phone; battery use over a long session.

### R12. Mochi and group bars (2026-09-24)
- Requests: the owner asked for the yellow blob to move when the phone tilts, and to become a playful, non-intrusive helper that congratulates finished tasks. Mid-build they added a second request: seeing a group's tasks took too many taps (Groups tab, then tile), so groups should sit folded on Today, open in place when tapped, and fold again when tapping anywhere else.
- Mochi: tilt physics with a spring (only while moving, paused when hidden), a shake detector, mouse-follow on laptops, seven moods, blinking, a speech bubble with cooldowns, hearts instead of repeated words, edge peeks at most every 3 minutes, a daily greeting, tap to chat, press and hold to nap, and settings switches. Full rules in DESIGN.md.
- Group bars: a "Groups" section on Today with one folded bar per group. One open at a time; tapping outside folds it, except the add box, pop-ups and Mochi. While a bar is open the add box targets that group. The Groups tab and group pages remain.
- Settings: the shield sheet is now "Settings" with a Mochi section. Mochi preferences travel inside backups.
- Rejected: Mochi speaking on every single completion (too chatty); a separate mascot per group; sound.
- Verified in the browser: the fold/unfold rules, adding into the open group, the finish-a-group reaction, the heart during cooldown, tilt response, tap reaction, peek, nap with Undo, laptop placement.
- Not verified: real gyroscope and shake on a physical phone, the iPhone motion permission prompt, battery impact over a long session.

### R11. Hosting on GitHub Pages (2026-09-23)
- Decision: public repo `smitmehta19/todo-app`, published at https://smitmehta19.github.io/todo-app/. The root page redirects to `app/`.
- Why: the owner's GitHub CLI was already signed in, it is free, it serves `https`, and there is no build step. Public is required for Pages on a free plan. The repo holds only code and design notes; tasks never leave the owner's device.
- Rejected: Netlify, Vercel and Cloudflare (each needs a new login, which I can't do for the owner); a Claude artifact (its sandbox blocks file downloads, which would break backups).
- Also added: a web app manifest and icons (a lemon jelly tile with a tick), so "Add to Home Screen" opens full-screen with its own icon; and a small network-first service worker, so the app opens offline. It caches the app only, never tasks.
- Git: the project now has its own repository at the project folder, so the home-directory repo is no longer involved (closes C1). Commits use the global identity, the owner's personal Gmail. `.claude/` and any `todo-backup-*.json` are ignored.
- Caveats: data lives per browser and per address. On iPhone, the home-screen app and Safari keep separate storage, so use one or the other. Any other site the owner later puts on `smitmehta19.github.io` shares the same storage origin; the key `todo.v1` is specific enough to avoid clashes.
- To update the live app: commit and push to `main`; Pages redeploys in about a minute.

### R10. Midnight blue instead of cocoa (2026-09-23)
- Problem: the owner said the dark brown ground looked like "poop colour".
- Cause: I chose a warm cocoa brown so the candy colours would feel like sweets on chocolate. The brown also bled into every card tint, ledge and shadow, so the whole screen read as muddy.
- Decision: every neutral moves to a midnight blue family (page #11142a, text #f7f5ff). The candy colours are unchanged. "Today" badges are now solid lemon, because see-through yellow over blue turned olive.
- Rejected: neutral charcoal (safe, but loses the playful night-time feel); keeping the brown with less tint (the base colour itself was the problem).
- Verified at phone and laptop size.

### R9. Squishy (2026-09-23), replaces R7
- Problem: the owner rejected R7 as "generic, not cute or fun enough", and asked for the impeccable skill to be used properly.
- Process: impeccable init interview, then `PRODUCT.md`. The owner answered four questions: the look was generic and not fun, the first screen should be Today, the main uses are quick capture and working through big projects, and it must be a responsive web app for laptop and phone. Four directions were presented (Sticker album, Squishy, Quest log, Clean and calm) and the owner chose Squishy. A direction contract sits at the top of the app's body. The build was followed by an independent reviewer and one fix batch, scored by the same reviewer, then a final three-item fix that I verified.
- Decision: groups are jelly tiles on a dark cocoa ground, with eight candy colours and dark ink on top. Every pressable element squashes into a ledge and springs back. Type is Baloo 2 and Nunito.
- Layout: on a phone there are three tabs (Today, Groups, Calendar) and a dock. On a laptop, Today is a left column and Groups or Calendar fill the right pane. Group pages open from their tile.
- Today screen: Late, Today, Coming up (7 days), Unsorted (no date and no group), Done today.
- Signature moment: finishing a group bursts jelly beans from its banner, the banner cheers, and a happy blob says "All done!".
- Reviewer findings fixed: empty-state icons blown up, flat brown task tiles, toast covering the dock, the laptop dock keeping a hidden group, the phone calendar hiding its list, clipped dock chips, a weak finish moment, an eyebrow label in the task sheet, sheet actions below the fold, done-title contrast (4.01:1 raised to about 6.5:1), heavy calendar ledges, a calendar class collision, and the burst coming from the wrong banner.
- Records: `PRODUCT.md` (product truth) and `DESIGN.md` (visual system).
- Not verified: real touch devices, Safari, notch safe areas, the phone share sheet, the import flow with a real file, a screen reader.

### R7. Redesign (2026-09-23), superseded by R9
- Problem: the user found the first simple build "very basic". Every group was the same grey card, so the group colours barely showed.
- Decision: each group is a solid slab of its own colour, like a notebook divider, with dark ink on top. Loose tasks sit on the page between them. Collapsed groups stack slightly, like cards in a wallet. A large date numeral heads the list with a one-line summary ("10 to do, 1 late, 4 due this week").
- Details: the add bar takes the colour of the group you are adding to. The task editor's header takes the task's group colour. The group editor previews the card as you type. First run offers one-tap groups (Marriage, Apply for jobs, Home). The calendar marks days with small bars in group colours.
- Motion: groups fold open and shut, a finished task is struck through and slides away, and finishing the last task in a group gives a small burst in that colour and says so. Sheets slide up and down. Everything turns off under reduced motion.
- Font: Bricolage Grotesque from Google Fonts. It is the one network dependency. Without a connection the app falls back to the system's rounded or standard font and still works. Google sees the font request.
- Palette: deep ink-blue page, eight group colours (coral, marigold, mint, sky, lilac, pink, lime, peach). Colour names from the first build are mapped to the new ones, so old backups still restore.
- Skills applied: impeccable (craft floor, bolder and delight guidance), design-taste-frontend (AI-tell checks), emil-design-eng (motion timing and easing), frontend-design (plan against the brief). impeccable's scripted steps cannot run here.
- Rejected: glass and blur effects, gradients, a light theme, adding features.
- Verified at 390 x 844: first run, list, collapsed stack, calendar, task, group and backup sheets, fold, complete with burst, group-finished message, adding into a group, add-bar colour. Found and fixed: invisible legend dots, too-deep card overlap, cramped group label in the task sheet.

### R1. Minimal scope
- Kept: add, complete with undo, edit, delete, remember the list, note, due date, calendar, done list, dark mode, groups.
- Cut: usage calendar, projects-as-sections, subtasks, labels, priorities, top three, do-dates, time blocks, durations, drag scheduling, capacity, rollover, quick-add parsing, command palette, search, notifications, reminders, recurring, trash, keyboard shortcuts, sign-in.
- Problem: the four demo directions were too hectic for a personal tool.
- Status: done. The old `demos` folder was deleted on the user's go-ahead (moved to the Recycle Bin).

### R2. One file
- `app/index.html` holds HTML, CSS and JS. No dependencies and no build. Since R7 it loads one web font, which is optional: without a connection it falls back to a system font.
- Rejected: Next.js and Supabase for now (only needed for accounts).

### R3. No accounts, backups instead
- Data lives in the browser's localStorage under `todo.v1`.
- Backup: "Save a backup file" writes `todo-backup-YYYY-MM-DD.json`. On phones it opens the share sheet so the file can go to Drive, iCloud or Files; elsewhere it downloads.
- Restore: validates the file, asks before replacing, keeps the previous data under `todo.v1.before-restore`, and offers Undo.
- Reminder: a banner appears when there are 5+ tasks and no backup, or the last backup is 7+ days old. It can be hidden until tomorrow.
- Safety: the app asks the browser to protect its storage from automatic clearing. If saved data is ever unreadable, it is parked under another key, never overwritten, and a red warning offers a restore.
- Limit: a web page cannot save files on its own schedule. Backups need one tap from the user.

### R4. Groups
- A group has a name and one of 8 colours. Tasks can be in one group or none.
- Add into a group with the + on its header, or pick a group in the add bar. The pick stays until changed, so several tasks can be added in a row.
- Deleting a group keeps its tasks without a group, with Undo.
- The calendar shows one dot per task in its group colour, with a legend.

### R5. Dark only
- User preference: dark as the constant theme. No light mode or toggle.

### R6. Phone use needs hosting (open)
- Opening an HTML file directly from a phone's file manager does not reliably keep data. It needs a web address (for example GitHub Pages, free). Data is tied to that address, so moving it later means backup then restore.
- Not done: publishing anywhere is outward-facing and needs the user's go-ahead.

### Verification (2026-09-23)
- Tested at 375 x 812 in the browser pane: add, group create from list and from picker, add into group, due dates, sorting, late styling, complete and undo, note editing, reload persistence, calendar dots and day list, export file contents, restore with undo, bad-file rejection, damaged-data warning. Console clean.
- Not tested: a real phone, Safari, the share sheet path.

---

## Superseded: demo phase (2026-09-11)

Phase 0: clickable visual demos only.

| # | Decision | Type | Status | Reversible |
|---|----------|------|--------|------------|
| A1 | Four directions, one self-contained HTML file each, phone-first | Architectural | Done | Yes |
| A2 | Shared engine and seeded data, assembled into each file from `demos/_src` | Architectural | Done | Yes |
| A3 | No Tailwind, no web fonts, no CDN: plain CSS variables and system font stacks | Architectural | Done | Yes |
| A4 | Demo shell wraps the app in a phone frame on desktop, full-bleed on phones | Architectural | Done | Yes |
| A5 | Shared calendar mechanics: drag from a tray, tap-then-tap, pull edge to resize | Architectural | Done | Yes |
| C1 | Git deferred until everything is ready; home-directory git repo must not be used | Critical | Open | n/a |
| C2 | Sign-in is a conventional placeholder pending the user's own design | Critical | Pending | Yes |
| C3 | "Today" frozen at Fri 11 Sep 2026 inside every demo | Critical | Done | Yes |
| C4 | Direction D breaks the shared task-block rule on purpose: deadlines only, no blocks | Critical | Done | Yes |
| C5 | Week views scroll horizontally on phones rather than squeezing seven columns | Critical | Done | Yes |
| D4 | Direction C: "Quiet" | Design | Done | Yes |
| D3 | Direction B: "Desk" | Design | Done | Yes |
| D2 | Direction A: "Ledger" | Design | Done | Yes |
| D1 | Direction D: "Pocket" | Design | Done | Yes |
| F1 | Zero-specificity button reset in each direction's CSS | Fix | Done | Yes |
| F2 | Mobile screen picker moved to top-right | Fix | Done | Yes |
| F3 | No top-level JS names that collide with `window` properties | Fix | Done | Yes |
| F4 | Class names must not collide across components in one file | Fix | Done | Yes |

## Architectural decisions

### A1. Four directions, self-contained files, phone-first
- Decision: `demos/direction-{a,b,c,d}/index.html`, each holding all screens, plus `demos/index.html` landing.
- Problem: the brief asked for three; the user added a fourth (simple, one list, cute) and said all are phone-first.
- Why: single files open by double-click, no server, no install.
- Rejected: a shared `assets/` folder (breaks "self-contained"); one file with all four directions (too large, muddles comparison).
- Changed: the brief's "left sidebar" became a desktop-only screen list beside a phone frame; on real phones it is a small pill top-right opening a sheet.
- Impact: 4 files of roughly 95 to 110 KB each. Reversible.
- Assumptions: a 390 x 844 phone frame is representative.
- Cost: none. Failed: nothing.

### A2. Shared engine and data, assembled from parts
- Decision: `demos/_src/` holds `data.js`, `engine.js`, `shell.css`, `template.html`, one `{x}.css` and `{x}.js` per direction, and `assemble.py`. The assembled HTML files are the deliverable.
- Problem: four files would otherwise duplicate 900 lines of engine and data; a data fix would need four edits.
- Why: the same seeded tasks in every direction means the user compares design, not data. Python is already on the machine; the script is a text concatenation, not a build tool.
- Rejected: writing each file by hand (drift); a bundler (out of scope by the brief).
- Impact: to edit a direction, edit `_src` and run `python demos/_src/assemble.py`. Editing an assembled file directly also works but the next assemble overwrites it.
- Reversible: yes, delete `_src` and keep the HTML.
- Open: whether `_src` should be removed before the eventual app repo is created.
- Failed: writing files through a Bash heredoc failed on a curly quote in Git Bash; switched to the editor tools and small Python patch scripts.

### A3. No Tailwind, no web fonts
- Decision: plain CSS custom properties; per-direction system font stacks.
- Problem: the brief allowed Tailwind via CDN, but "opening the file must just work" includes offline and on a phone.
- Why: no network dependency; full control of typography and tokens; smaller files.
- Rejected: Tailwind CDN (needs network, generic look), Google Fonts (needs network).
- Impact: Windows renders Segoe UI where a rounded (D) or humanist (B) face was intended, and Palatino Linotype for C's serif. Fonts are the first thing to revisit when the real app is built.

### A4. Demo shell
- Decision: neutral grey shell, numbered screen list, theme toggle, reduced-motion toggle, phone frame. Alt + arrow keys change screens; the list is keyboard-navigable.
- Problem: the brief asked for a left sidebar and a light/dark toggle; phone-first made a sidebar impossible inside the app.
- Why: the shell must not compete with the direction's own design.
- Rejected: a tab strip above the app (adds chrome to every screenshot).
- Detail: the engine preserves the focused input, its caret and every scroll position across re-renders, so typing in a composer while the app re-renders does not lose the keystroke.

### A5. Shared calendar mechanics
- Decision: one `gridInteractions` helper in the engine. Markup contract: columns carry `data-col`, draggable items `data-tray`, blocks `data-block` with a `data-resize` handle.
- Problem: three directions need drag-to-schedule, tap-then-tap on touch, and resize-writes-duration.
- Why: identical behaviour in A, B and C, styled differently; drop snaps to 15 minutes and creates a 30-minute block; resize writes the duration back onto the task.
- Rejected: HTML5 drag and drop (no touch support, ugly ghost).
- Verified: tap-then-tap and drop tested in the browser on A; the same code runs in B and C.

## Critical decisions

### C1. Git deferred
- The project folder sits inside a git repository rooted at the user's home directory. Committing there would track the whole home folder. The user chose "local first, git once everything is ready". No `git init` has been run. When the time comes: `git init` inside the project folder, then commit.

### C2. Sign-in placeholder
- Pending the user's own design, which has not been shared. Each direction ships a clean conventional email + password + Google form, with inline validation errors. Each screen says so in its footer. Flag: replace when the design arrives.

### C3. Frozen today
- `TODAY = '2026-09-11'` in `data.js`, and the demo clock is 10:40. All overdue, due-today and missed-block states derive from it. Reversible by editing one constant.

### C4. Direction D is deadline-only
- Pocket has no do-date, start or duration, so it has no blocks and no capacity warning. Its edge-states screen says so in place of the "empty project" and "capacity" states. Overdue in Pocket offers to move the deadline, which the shared rule forbids for A to C. This is deliberate: D is the "simple" opinion. The user should decide whether that simplification is acceptable.

### C5. Week views scroll horizontally on phones
- Seven columns in 390px were unreadable in the first Ledger pass. A, B and C now put the deadline strip and the grid in one horizontal scroller (A: 640px wide, B: 112px columns, C: 176px columns) that opens on today. The brief's "deadline strip across the top, blocks in the grid below" holds; the grid is simply wider than the phone.

## Everything else, newest first

### 2026-09-11. F4. Class name collisions inside one file
- B's calendar block class `.blk` also matched a pill tag with class `tag blk`, making the tag absolutely positioned across the row. Renamed the tag to `tag when`. A's keyboard help overlay `.help` collided with a form help line; renamed to `.kbd-help`. Rule going forward: one meaning per class name per file.

### 2026-09-11. F3. Reserved global names
- A top-level `function top()` in Direction A threw "Identifier 'top' has already been declared" because classic scripts declare globals on `window`, where `top` is read-only. Renamed to `topbar`. A check for `top, parent, name, length, status, close, open, location, history, event, origin, frames, self` now runs after every assemble.

### 2026-09-11. D4. Direction C, Quiet
- World: cool off-white and ink, one deep teal, a book serif (Palatino stack) at 17px, hairlines only, pill buttons as the only radius. Motion: 400ms fades and a strike-through that draws itself.
- Structure: one column. "Three for today" is the largest type on the list screen. Projects are italic headings. Someday collapses to one sentence. Tasks are sentences with a trailing italic phrase for the date ("due Tuesday, at 9:30am for 2 h"). A "plan" word after an unscheduled task arms it; Today's timeline is the drop target. Navigation is four serif words at the bottom and a small menu top right.
- Detail is prose with inline controls ("It is done · not done yet, and one of today's three. Priority P1 P2 P3 P4 in Inbox, Marriage...").
- Rejected: cards, icon tabs, a grid on the list screen.
- Open: whether inline-prose editing in the detail screen is too clever for daily use.
- Failed: agenda block rows overlapped their time label; fixed with a wider column.

### 2026-09-11. D3. Direction B, Desk
- World: pale sage ground, white pills with soft shadows, one deep green for actions, project colours only as dots and block tints. Humanist sans at 15px. Motion: the plan sheet's drawer curve, pill press, block landing.
- Structure: project chip filter row, pills with a drag grip on the left, a "Plan today · 3h 45m of 8h" sheet docked at the bottom of the list that lifts to reveal today's timeline as a drop target. Week shows 3.5 days at a time and scrolls. Month uses colour bars per deadline. Duration is a stepper in the detail.
- Rejected: a separate calendar tab as the only way to schedule; a floating add button (the app bar + is enough).
- Open: whether the grip-only drag is discoverable enough on touch; the sheet's helper line explains it.
- Failed: the block tag class collided with the calendar block class (F4); the first pass had missed blocks rendering as a red band across the pill.

### 2026-09-11. D2. Direction A, Ledger
- World: cool white paper, ink, one cobalt accent, red ink for anything late. System sans for words, monospace tabular columns for project letter, deadline and duration. 38px rows, hairlines, 3px radii, no motion beyond a 120ms toast.
- Structure: top line with project and count; rows with a keyboard cursor (j/k, x, e, n, /, c, t, g+letter, ?, Ctrl+K); five text tabs at the bottom; a key-hint strip. Today is a split: list above, timeline below. Week is a real grid with a deadline strip and an unscheduled tray.
- Rejected: icons in rows; cards; hover-only affordances (everything also works by touch and keyboard).
- Open: density on a phone is high by design; the user should judge whether it is too high.
- Failed: `top()` global collision (F3); focus ring drawn around the whole list; key strip wrapping; month grid clipping its last column (missing `min-width:0`); help class collision (F4). All fixed.

### 2026-09-11. F2. Mobile screen picker moved to top-right
- The bottom-centre pill collided with Pocket's composer. Now a small pill top-right. Applies to all directions.

### 2026-09-11. F1. Zero-specificity button reset
- `.app button{color:inherit}` outranked `.btn`, so primary buttons rendered ink-on-ink. Replaced with `:where(.app button)`. Same pattern in every direction's CSS.

### 2026-09-11. D1. Direction D, Pocket
- World: butter ground, paper rows, tomato action, teal done, rounded type, 20px radii, a scribble stroke to finish a task, a mascot named Pip with four moods.
- Screens: the fourteen plus "Days you showed up" (usage calendar with no streaks).
- Interactions that really work: scratch-to-complete with undo, composer with date parsing, quick actions sheet with keyboard navigation, week and month day picking, task detail editing, slipped-date prompt, trash and restore, theme toggle, reduced motion.
- Contrast: accent text uses a darker tomato than the button fill so both pass AA on butter; dark mode has its own lighter accents.
- Rejected: purple or gradient looks (generic), a bottom tab bar (too much chrome with the composer), streaks (out of scope).
- Assumption: "one main category" means no projects at all, so Pocket flattens the shared data and drops subtasks and Someday.
- Open: whether the mascot is welcome or too much.
- Failed: primary button text invisible (F1); search snippets scattering inside an inline-flex span; both fixed.

### 2026-09-11. Skills that fired
- impeccable, design-taste-frontend, emil-design-eng, frontend-design were all installed and loaded. design-taste-frontend declares product UI out of its scope, so only its list of AI tells was applied. impeccable's node runtime is not available in this session, so its scripted concept roll and reviewer agents did not run; its craft floor and Operate guidance were applied by hand, and each file carries the direction contract as an HTML comment at the top of the body.

### Verification done
- Every screen of every direction was rendered in a browser at 420 x 860 in light mode and checked by screenshot; D was also checked in dark mode and in the desktop frame. Console was clean after fixes. Interactions exercised by hand: scratch-to-complete and undo (D), tap-then-tap scheduling and drop (A), plan sheet opening (B).
- Not verified: real touch drag on a physical phone; Safari; contrast measured by tool (values were chosen by calculation, not measured).

### Versions pinned
- None. No dependencies. Python 3 (already installed) is used only by the optional assembler.

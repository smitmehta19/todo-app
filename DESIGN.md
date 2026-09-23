# Design

The visual system of the to-do app, recorded from the built file `app/index.html`. Direction: **Squishy**, chosen by the owner on 2026-09-23.

## Idea

Tasks are soft toys you press. Every group is a jelly-coloured tile, and every tap squashes and springs back. Charm lives in the moments that repeat (adding, finishing) and never slows them down.

## Colour

Dark only. Midnight blue ground, near-white text, eight candy colours for groups with dark ink on top. (The first version used a dark cocoa brown; the owner rejected it as muddy, so warm browns are out.)

| Token | Value | Use |
|---|---|---|
| `--bg` | #11142a | Page, inset fields |
| `--surface` | #1b1f3b | Neutral tiles, dock, sheets |
| `--surface2` | #262b4d | Chips, raised selection |
| `--ledge` | #080a18 | Pressable ledge under neutral elements |
| `--text` / `--cream` | #f7f5ff | Text; the primary action colour |
| `--mute` | #bcc0e0 | Secondary text, placeholders, done titles |
| `--faint` | #9a9fc6 | Counts and hints only |
| `--ink` | #1b1533 | Text on candy colours |
| `--late` | #ff8f80 | Late tasks |
| `--soon` | #ffd66b | Due today (solid badges with ink text) |

Group colours: strawberry #ff7a9c, tangerine #ff9a52, lemon #ffd54f, apple #9be15d, mint #4fdcc0, blueberry #6fa8ff, grape #b48cff, bubblegum #ff9de2. Tasks without a group use a neutral #8f95c0, and the "No group" tile is #363c6b.

Rules:
- Never put a see-through warm colour (yellow, orange) over the blue ground. It mixes into olive or brown. Use it solid, with ink text.
- Candy colours carry whole objects (tiles, banners, active chips), never thin accents alone.
- Text on candy is always `--ink`. Ink on every candy is at least 6.6:1.
- A task tile is tinted about 12% with its group colour, and its ledge is mixed from the same colour.

## Type

- Display: **Baloo 2**, weights 700–800, for headings, counts, buttons and tabs. The page title is 48px at -0.025em.
- Body: **Nunito**, weights 600–800, at 16–17px.
- Both come from Google Fonts. Fallbacks are `ui-rounded`, then the system font, so the app works offline.
- Numbers use tabular figures. Headings use `text-wrap: balance`.

## Material: jelly

The one material, used by the `.jelly` class:
- A top highlight gradient and a small gloss streak.
- A ledge underneath: a zero-blur shadow mixed from its own colour, plus a soft drop shadow.
- On press, the element moves down into its ledge and squashes (`translateY(5px) scale(1.015,.975)`) in 90ms.
- On release it springs back over 380ms with `cubic-bezier(.34,1.56,.64,1)`.

The ledge exists to show the press. Plain surfaces that are not pressable, and calendar days with nothing on them, stay flat.

Radii: tiles 24–30px, chips 14px, fields 18px, sheets 32px.

## Layout

- **Phone, under 900px:** one pane at a time, switched by three jelly tabs (Today, Groups, Calendar). The add dock sits above the tabs.
- **Laptop, 900px and up:** Today is a fixed left column (360–410px) with the dock beneath it. The right pane switches between Groups and Calendar with a segmented control. The phone tabs are hidden.
- The dock's group and date follow what is on screen. A group page presets that group, and the calendar presets the selected day.

## Motion

- **Focal moment:** finishing the last task in a group. Its banner does a cheer squash, 34 jelly beans burst from it in the group colour, and the group shows a happy blob with "All done!".
- **Completing a task:** the bubble squishes and fills, then the tile drops away (about 600ms in all). Undo is always offered.
- **Adding a task:** the new tile drops in with a spring.
- **Opening a group:** the tile grows into the group page (View Transitions, where supported).
- **Sheets:** slide up on a phone, bloom on a laptop, and exit faster than they enter.
- **Reduced motion:** all of the above is switched off, and state changes still show.

## Mochi, the helper

The yellow blob is Mochi, a small jelly character who makes finishing things feel noticed and never gets in the way.

- **Home:** sits on the top edge of the add box, half tucked behind it, breathing and blinking. Hidden whenever a big empty-state blob is already on screen, so there are never two.
- **Tilt:** on phones Mochi (and every blob) leans, slides and squashes with the phone's tilt; the face and shine drift separately so it reads as jelly. A shake makes Mochi dizzy. On laptops the eyes follow the mouse. iPhones need one tap on Mochi to allow motion.
- **Moods:** happy, wow, love (heart eyes), wink, sleepy (late at night), dizzy, worried.
- **When Mochi speaks:** a short cream bubble, 2.6–5 seconds, tap to dismiss.
  - Finishing a task: a hop and a cheer. Within 7 seconds of the last line, a floating heart instead of more words.
  - Three in a row: a streak line. Finishing a group or clearing today: a spinning jump and a special line.
  - Adding a task: a nod, and sometimes "Got it!".
  - Once a day: a greeting that mentions today's count.
  - After 45 seconds of quiet, at most every 3 minutes and only half the time: Mochi peeks in from a screen edge with a small line, then goes home.
- **Never:** while a sheet is open or while you are typing; never about deleting; never guilt.
- **Control:** tap for a chat, press and hold to nap (with Undo), switches for Mochi and tilt in Settings.
- **Reduced motion:** no hops, tilt or peeks; lines still appear.

## Components

- Group bars on Today: each group is a folded jelly bar (name, next due, count left). Tap to open it in place, tap anywhere else to fold it. While one is open, new tasks go into that group.

- Task tile: bubble, title, then chips for due date, group tag and note.
- Group tile: name, big count, next due.
- Group banner: name, counts, Add and Edit buttons.
- Dock: chips (group, Today, Date) and a field with a send button in the group colour.
- Calendar day: date plus coloured beads; today is lemon jelly.
- Blob: the mascot for empty and all-done states.
- Toast: cream pill with Undo, placed above the dock on phones and bottom-right on laptops.
- Sheets: task editor with its colour band from the group, group editor with a live preview tile, a group picker, and backup and restore.

## Don'ts

- No eyebrow labels above headings.
- No grey-card lists, and no flat colour slabs without the jelly material.
- No gradient text or glass.
- No emoji as icons. Icons are the inline stroke set, weight 2.4, with round caps.
- No light theme.

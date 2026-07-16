# TABI — Product & Screen Specification
### A calm, journal-styled personal productivity app for breaking life goals into small daily "chunks"

---

## 1. APP CONCEPT (One Paragraph)

Tabi is a minimal, distraction-free task app built around three nested layers: **Areas** (broad life categories like Career, Health, Home), **Journeys** (specific goals/projects within an Area, e.g. "Design Portfolio"), and **Chunks** (small, bite-sized tasks within a Journey, e.g. "Write portfolio introduction"). There is **no calendar and no scheduling system**. Every day, the user manually pulls a few Chunks from their Journeys into a single list called **Today**, works through them, and checks them off (strikethrough) as they're completed. The whole app is wrapped in a warm, tactile "paper and desk" visual metaphor — torn notebook paper, paperclips, binder clips, stamps, sticky notes — paired with a persistent ambient "Room" image (a cozy desk scene) and an optional focus timer and background music, to make the act of planning feel calm and analog rather than corporate and naggy.

**Core philosophy: this is intentionally simple.** No notifications, no calendar/date-scheduling, no collaboration, no gamification beyond a simple progress bar and a wax-stamp logo. Do not introduce features beyond what is specified below.

---

## 2. INFORMATION ARCHITECTURE / DATA MODEL

```
Area (e.g. "Career")
 ├─ color (assigned automatically or chosen from palette)
 ├─ icon (folder icon, colored)
 ├─ note (single freeform journal-style text field, optional)
 └─ has many Journeys

Journey (e.g. "Design Portfolio")
 ├─ belongs to one Area
 ├─ icon (small square glyph, e.g. briefcase, sofa, book)
 ├─ color (inherited/matches its Area or chosen at creation)
 ├─ title
 ├─ description ("Building a strong case study based portfolio.")
 ├─ "About this journey" long description
 ├─ deadline (date, optional — display only, not a calendar feature)
 ├─ created date (auto-stamped, displayed, not editable)
 ├─ progress = completed chunks / total chunks (e.g. "18 / 42 chunks")
 └─ has many Chunks

Chunk (e.g. "Write portfolio introduction")
 ├─ belongs to one Journey (which belongs to one Area)
 ├─ title
 ├─ estimated duration (e.g. 30m, 45m) — used to pre-fill the focus timer
 ├─ priority: High / Medium / Low (colored dot: red / orange / blue)
 ├─ completed: boolean (checkbox)
 └─ isInToday: boolean (whether it's currently pulled into the Today list)

Today (not a stored entity — a filtered view)
 └─ shows all Chunks across all Journeys where isInToday = true,
    for the current real-world day only. No future/past day browsing.
    No date picker. Resets/clears conceptually each new day (chunks
    must be re-added; there is no carryover automation — keep this
    simple and manual per the "no calendar" rule).

Room (ambiance entity)
 ├─ name (e.g. "Warm Studio")
 ├─ background image (user-uploaded photo OR default preset)
 └─ user can own multiple Rooms and switch between them

Profile / Settings
 ├─ display name (optional; if set, replaces "My Profile" / shows
 │   instead of default label in the top-right account chip)
 ├─ avatar photo
 ├─ Rooms management: add a Room (name + image upload), delete, switch
 └─ Music management: add a music track (upload audio file), see a
     list of uploaded tracks alongside default built-in tracks,
     select which one plays
```

---

## 3. GLOBAL LAYOUT (applies to every screen)

The entire app lives inside a **single fixed window** styled like a physical desk surface:

- **Outer frame:** thick, almost-black rounded-rectangle border (like a wood-grain dark frame), about 16–20px thick, rounded corners ~24px radius. This frame is always visible around the whole app and never scrolls.
- **macOS-style traffic light dots** top-left: red, yellow/orange, green filled circles, ~10px diameter, evenly spaced, vertically centered in the top bar.
- **App name "Tabi"** in a serif/typewriter-style bold font immediately right of the dots, with a small dropdown chevron (˅) beside it (app menu affordance, non-functional dropdown is fine).
- **Top bar background:** same aged-paper texture as the rest of the app, full width, sitting above the main paper sheets, with a thin dark hairline separating it from the content below.
- **Top-right cluster**, always present on every screen, right-aligned:
  1. A pill-shaped "music" control: musical-note icon, italic label text "No music playing" (or current track name), small dropdown chevron — this opens the music source selector.
  2. A circular play button (▶, filled triangle in red/maroon ink color) immediately to the right of the music pill — same row.
  3. On screens **other than Today**, an additional small circular "+" button appears between the play button and the profile chip (visible in Areas, Journeys, Room, Journey-detail screens) — this is the global quick-add (add Journey) shortcut.
  4. **Profile chip**: circular avatar photo (small grayscale/sepia portrait), the user's name label ("Adhithya" — or "My Profile" if no name set in Settings), and a dropdown chevron. Clicking opens Settings.

- **Left sidebar (persistent, all screens):** a vertical stack of 4 tab cards, each styled as a stacked index-card / file-tab:
  1. **Today** — icon: small calendar-with-star/pin icon
  2. **Areas** — icon: folder icon
  3. **Journeys** — icon: list/clipboard icon
  4. **Room** — icon: armchair icon
  - Each tab card has a thin colored left edge stripe (red, yellow, blue, green respectively — these are constant per-tab accent colors, NOT tied to area colors) that looks like the colored edge of a manila folder peeking out from behind the card above it.
  - The **active/selected tab** is visually "pulled forward": slightly larger drop shadow, sits on top of the stack, and shows a small solid red/maroon dot in its top-right corner (like a wax seal marking "you are here" / unread marker).
  - Tabs are stacked with a slight overlap (like a tabbed card index), each subsequent card peeking out from behind/below the previous one.
  - Below the tab stack: a **paperclipped note card** (see Section 5 — recurring motif) pinned at an angle, containing a static motivational quote: *"Discipline is choosing between what you want now and what you want most."* with a hand-drawn red underline/scribble beneath it. This note is decorative and identical on every screen.
  - At the very bottom of the sidebar: a circular **wax stamp logo** — "TABI" arched across the top, a simple triangular mountain-range glyph with a dotted path in the center, "KEEP GOING" arched along the bottom, all rendered in a faded brick-red ink, looking stamped/rotated slightly off-axis, with a distressed/grainy texture.
  - Bottom-left corner, below the stamp: two small circular icon buttons side by side — a **gear/settings icon** and a **question-mark/help icon**.

- **Texture treatment (applies everywhere):** every panel uses a warm beige/khaki **aged paper texture** with visible flecks/grain, soft mottling, and slightly darker vignetting at edges — never a flat or pure-white background. Paper edges are very subtly ragged/deckled, not perfectly straight-cut, especially on the center "page."

---

## 4. SCREEN-BY-SCREEN SPECIFICATION

### 4.1 TODAY (default/home screen)

**Three-column layout:** Left sidebar (per Section 3) | Center "page" | Right column.

**Center column — styled as a single torn notebook page clipped at the top:**
- A black **binder clip** (realistic, dimensional, dark metal with two folded silver tabs) clamps the top-center edge of the page to the screen, as if the page is bound into a notebook.
- Page has visible **3-hole-punch holes** running down its left edge (3 holes, evenly spaced top/middle/bottom), reinforcing the "notebook paper" metaphor.
- Page edges are torn/deckled, not perfectly rectangular (slightly rough at top corners).

**Header row inside the page:**
- Large title **"Today"** in bold black serif/typewriter font, top-left, largest text on the page.
- Directly below it, smaller red/maroon italic text: **"4 chunks selected"** (dynamic — reflects count of chunks currently in Today) with a hand-drawn red underline scribble beneath it.
- Top-right of the header: a **circular date stamp** graphic — red ink, circular border, containing three stacked lines of text: day abbreviation ("WED"), large day number ("24"), month+year ("JUN 2024"). Styled like a vintage rubber postmark stamp, rotated very slightly.
- Beside the date stamp (further right): a small **sticky note** taped/paperclipped at a slight tilt, handwritten-style italic text: *"Focus on progress, not perfection."* — a rotating/random daily affirmation, paperclip icon at its top-left corner attaching it to the page.
- A thin horizontal divider line beneath the whole header block, spanning the page width.

**Chunk list (the core of this screen):**
A vertical list of rows, each row = one Chunk pulled into Today. Each row contains, left to right:
1. **Checkbox** — square outline checkbox. Unchecked = empty bordered square. Checked = filled with a red checkmark/X-tick mark and a red border.
2. **Chunk title** — bold black text. When checked, the title text gets a **strikethrough line** and visually reads as "completed" (still legible, just struck through — do not gray it out completely or remove it from the list).
3. **Metadata sub-line** directly under the title, smaller gray text: `Journey: [Journey Name]  •  Area: [Area Name]` — always shows the parent Journey and parent Area for context, separated by a bullet.
4. **Right-aligned duration** — small clock icon + duration text, e.g. "🕐 30m".
5. **Right-aligned priority tag** — a small colored dot + label: red dot "High", orange dot "Medium", blue dot "Low".
- Each row is separated from the next by a thin hairline divider.
- Rows have generous vertical padding (comfortable, journal-like spacing, not cramped).
- List shows a mix of checked (struck-through, at the top) and unchecked chunks; completed ones float to/stay at the top in the order they were checked (matches screenshot: 2 checked items first, then 4 unchecked).

**Bottom of the list:**
- A dashed-border rounded rectangle button, full width of the list area: a circled "+" icon and the label **"Add a chunk to today"**. Clicking this opens a picker (modal or expandable list) where the user selects from existing Chunks across all Journeys to pull into Today. This is the ONLY way chunks enter Today — there is no free-text "quick add task" on this screen; chunks must already exist under a Journey.

**Right column (persistent mini Room + mini Journeys list — see Section 5 for full shared spec):**
- Top: mini Room card showing the current Room background photo, with a header row "🪑 Room ˅" dropdown and a "Change Room 📎" button top-right of the image.
- A "Current Chunk" focus card overlapping the bottom of the Room image: shows label "Current Chunk", the chunk's name (defaults to the next unchecked high-priority or first chunk — shown as "Review typography & spacing"), its duration/priority, a large countdown display ("30:00") and a red circular play (▶) button to start the focus timer right from here.
- Below that: "📎 My Journeys" card listing each Journey as a compact row (icon-colored left bar, folder icon, Journey name, thin progress bar, "x / y chunks" count, colored status dot). 
- Bottom of this card: dashed "+  Add Journey" row.

---

### 4.2 AREAS

**Center column — same torn-page-with-binder-clip-and-holes treatment, but note the binder clip is NOT shown on this screen (only a paperclip motif persists); page punch-holes still present down the left edge.**

**Header:**
- Large bold title **"Areas"**.
- Subtitle directly below in red italic with hand-drawn underline: *"Focus on the areas that shape your life."*
- Top-right: a sticky note (paperclipped, tilted) reading *"Small improvements, every day."*
- Beside the sticky note, a circular red ink stamp graphic with three stacked words: "FOCUS / BUILD / GROW" (this stamp graphic recurs identically on Areas, Journeys, and Journey-detail screens — it is a constant decorative brand stamp, not dynamic).
- Thin horizontal divider beneath header.

**Area list:**
A vertical stack of rows, each one rectangular card representing one Area:
- **Left:** a colored folder icon (the folder glyph itself is rendered in a flat color matching that Area's assigned color) inside a rounded-square colored chip/background tinted to match.
- Next to the folder icon: a small solid colored dot, then the **Area name** in bold black text (e.g. "Career", "Home", "Health", "Personal", "Creative", "Learning").
- **Right side of the row:** italic gray placeholder text **"Add note..."** — this is a direct inline editable field. Clicking it turns it into an active text cursor right there inline (no popup, no modal, no visible text-field border/box) — typing feels like writing directly on the journal page. Once a note exists, it replaces the placeholder text in the same italic journal-handwriting style.
- Far right: a small **chevron-down (˅)** icon — clicking expands the row to reveal the Journeys nested under that Area (in place, accordion-style) and/or simply navigates into a filtered Journeys view. Keep this simple: expanding reveals nothing more than a quick peek; the canonical place to browse Journeys/Chunks in depth is the Journeys tab.
- Each Area row uses one color from the fixed palette in this fixed order: Career = red/maroon, Home = blue, Health = green, Personal = yellow/gold, Creative = purple, Learning = brown/tan. (This mapping should stay consistent everywhere the Area appears — Today's metadata line, Journeys' Area references, etc.)
- Rows are separated by thin hairline dividers, consistent spacing.

**Bottom of the list:**
- Dashed-border button: circled "+" and label **"Add new area"** — clicking opens an inline new row where the user names the Area and assigns/auto-receives a color and folder icon.

**Right column:** identical persistent Room + My Journeys panel as every other screen (see Section 5), except the Room card here shows the Room's name "Warm Studio" with a dropdown chevron (user can have named, switchable Rooms) instead of the generic "Room" label seen on Today.

---

### 4.3 JOURNEYS (list view)

**Center column — torn page, punch holes, no binder clip visible.**

**Header:**
- Large bold title **"Journeys"**.
- Subtitle in red italic with underline: *"Paths of focus. Progress over time."*
- Same sticky note ("Small steps, meaningful progress.") + "FOCUS / BUILD / GROW" stamp combo, top-right, as a recurring decorative pair.

**Add row:**
- Directly under the header, a dashed-border full-width button: circled "+" and label **"Add new journey"**.

**Journey list:**
Each Journey is a card row containing:
- **Left:** a colored rounded-square icon tile (solid fill in the Journey's/Area's color) containing a simple white glyph representing the journey type — briefcase for Design Portfolio (Career), sofa/armchair for Apartment Setup (Home), a Japanese character "あ" for Learn Japanese (Learning), a dumbbell for Health Routine (Health), an open book for Read More Books (Personal/Learning).
- **Title** in bold black, e.g. "Design Portfolio".
- **One-line description** in gray beneath the title, e.g. "Building a strong case study based portfolio."
- **Right side, top:** progress fraction text, e.g. "18 / 42 chunks", plus a small colored dot matching the Journey's color, plus a right-pointing chevron (›) indicating it's tappable to drill in.
- **Right side, bottom:** small calendar icon + "Created on 12 Jun 2024" — a static creation-date stamp, NOT an editable/interactive calendar feature.
- Rows separated by hairline dividers, comfortable padding, rounded card corners, very subtle card background tint (barely-there paper card on paper page).

**Right column:** same persistent Room + Current Chunk + My Journeys panel. Note: on the Journeys/Areas/Room/detail screens, the My Journeys panel list shows ALL journeys (5, in this case) rather than a top-3 preview, and includes a "View all journeys →" link at the very bottom when on the Journeys tab itself.

---

### 4.4 JOURNEYS → JOURNEY DETAIL (clicking into a single Journey)

When a Journey row's chevron/card is clicked, the **right column transforms**: instead of showing the Room/My Journeys panel, it now shows the **selected Journey's full detail** as its own "page," styled like a manila folder tab popping forward (a black binder clip clamps its top edge; small folder-tab triangles peek from behind top corners; a pencil with a pink eraser is illustrated lying diagonally along the bottom-right edge of the page, decorative only).

**Journey detail page contents (top to bottom):**
- Small label in red caps: the parent **Area name**, e.g. "CAREER".
- Large bold title: the **Journey name**, e.g. "Design Portfolio", with a red hand-drawn underline beneath it.
- To the right, the same circular "FOCUS / BUILD / GROW" red stamp graphic, rotated.
- **"Deadline"** label (small gray caps) with the date beneath in red bold, e.g. "Jul 1, 2024." (Display-only field, set once at Journey creation — not a calendar/scheduling system.)
- **"About this journey"** label (small gray caps) with a longer descriptive paragraph beneath in black, e.g. "Create a portfolio that showcases my best work and gets me hired."
- Horizontal divider.
- **"Chunks"** section label (red) with the total chunk count right-aligned, e.g. "6".
- A vertical list of all Chunks belonging to this Journey, each row:
  - square checkbox (unchecked/checked state, same style as Today)
  - chunk title in black
  - a "···" (more options) icon far right of each row, for per-chunk actions (e.g. edit, delete, send to Today)
- Bottom of the page, a row of three controls:
  - a small **document/notes icon** (bottom-left) 
  - a small **trash/delete icon** beside it (delete this Journey)
  - a prominent bordered button, right-aligned, red text on cream background: **"KEEP GOING"** — this is the call-to-action that likely jumps the user back to Today or opens the "add chunk to today" flow seeded from this Journey.

---

### 4.5 ROOM

**This screen drops the 3-column layout.** It is a single, full-bleed, large photographic rendering of the Room: a warm, sunlit cozy desk/study scene — wooden desk, an open laptop showing a mountain-lake wallpaper, a stack of books (one titled "The Art of Focus"), an open journal with a pen, a ceramic mug of coffee, a black articulated desk lamp glowing warm, a wall poster reading "Discipline creates freedom.", a tall bookshelf with books, potted plants, and a lit candle, a window to the left with daylight and greenery outside, a wooden chair pulled up to the desk.

- The left sidebar (Today/Areas/Journeys/Room tabs, quote note, stamp, settings/help icons) remains visible, overlaid in its normal position, but here it sits directly on top of the photographic room background rather than on a separate paper panel — same card-stack tab styling.
- Top bar (music pill, play button, "+" button, profile chip) stays in its normal position, overlaid at the top of the photo.
- Top-right of the photo, below the top bar: a **"Current Journey"** card overlay — label "Current Journey" in red italic, the active chunk's name below it in black ("Review typography & spacing"), and a metadata line "🕐 30:00  •  🟠 Medium".
- **Bottom-right of the photo:** the **focus timer control**, in its fullest/expanded form on this screen:
  - A large countdown display, e.g. "30:00" in bold red.
  - A circular red play (▶) button beside it.
  - Below that, a horizontal row of **duration preset chip buttons**: 15m, 20m, 30m, 45m, 60m — the currently selected duration (30m in the example) is shown as a solid filled red/maroon chip with cream text; unselected durations are plain cream outlined chips with dark text.
- This screen is purely the immersive ambiance + timer screen. There is no list content here — its entire job is to be a calm focus-mode visual with a working countdown timer that can be started/stopped and re-configured by duration.

---

## 5. RECURRING SHARED COMPONENTS (appear identically across multiple screens)

### 5.1 Right column — "Mini Room + My Journeys" panel
Present, unless replaced by a Journey-detail panel, on Today, Areas, Journeys, and conceptually available wherever space allows. Structure top to bottom:
1. **Mini Room card**: rounded rectangle photo of the current room (smaller crop of the same Room photo used on the full Room screen), header overlay top-left reading "🪑 Room ˅" (or the Room's custom name + dropdown, e.g. "Warm Studio ˅" once named) and a "Change Room 📎" pill button top-right, both sitting on a small translucent cream bar across the top of the photo.
2. **Current Chunk card**, overlapping/anchored to the bottom edge of the mini Room photo (visually looks clipped onto it, with a folded paper-corner triangle effect at its top-right corner): "Current Chunk" red italic label, chunk title in bold black, duration + priority dot/label beneath, large countdown timer text on the right, red circular play button.
3. **My Journeys card**, separate paper card below, paperclip icon at its top-left corner, header "My Journeys": a list of Journey rows — each with a thin colored vertical bar on its far left edge, a folder icon, Journey name (in its assigned color on Today/Areas previews, plain black on Journeys/detail screens — match the screenshots: bold colored text for the 3-item Today preview, black text with separate color dot for the full Journeys-tab version), a thin horizontal progress bar beneath the name, the "x / y chunks" fraction, and a small trailing colored status dot.
4. Bottom of the My Journeys card: a dashed "+ Add Journey" row (and, only when on the Journeys tab itself, a "View all journeys →" link beneath the full list instead of/in addition to the add row).

### 5.2 Sticky note / paperclip motif
A recurring decorative device: small rectangular "note card" pieces of paper, slightly rotated (2–6°), with a gray paperclip icon clipped to their top-left corner, containing short italic handwritten-style text. Used for: the static sidebar quote, the daily affirmation on Today, the tagline notes on Areas/Journeys headers. Always cream/aged paper, soft drop shadow beneath to suggest it's physically resting on top of the page below it.

### 5.3 Date / brand stamps
Two distinct circular rubber-stamp graphics, both rendered in a faded brick-red ink with a slightly distressed/grainy edge, both subtly rotated off-axis:
- **Date stamp** (Today only): circular border, three lines — weekday abbreviation, large day number, "MON YYYY".
- **"FOCUS / BUILD / GROW" stamp** (Areas, Journeys, Journey detail): circular/square-ish bracket border, three stacked words.
- **App logo stamp** (sidebar, all screens): circular, "TABI" arched on top, mountain-range + dotted trail glyph in the middle, "KEEP GOING" arched on the bottom.

### 5.4 Checkbox / chunk-row pattern
Used identically on Today and Journey-detail: square outline checkbox (empty = unchecked; filled with red checkmark/cross + red border = checked), title text immediately right (struck-through once checked), optional metadata line beneath, optional right-aligned duration/priority tags depending on context (Today shows them, Journey-detail's simplified inner list keeps just the "···" menu instead).

---

## 6. INTERACTION / LOGIC RULES

1. **Chunks live under Journeys; Journeys live under Areas.** A Chunk cannot exist without a parent Journey; a Journey cannot exist without a parent Area.
2. **Today is a non-persistent, manually-curated cross-section**, not a calendar day. The user explicitly adds existing Chunks into Today via "Add a chunk to today." There is no auto-scheduling, no recurring tasks, no due-date-based surfacing. (Keep it manual and simple, per the product brief.)
3. **Checking a Chunk's checkbox** (on Today or inside a Journey) marks it complete: strikethrough applied to its title text, and it increments the parent Journey's completed-chunk count (and thus its progress bar / "x / y chunks" fraction everywhere that Journey is referenced — Today metadata stays as plain text, Journeys list, My Journeys panel, Journey detail header).
4. **Notes on Area rows are inline-editable directly on the page** — clicking "Add note..." immediately places a text cursor in that same spot with no modal, no bordered input box, no separate "save" button; it should look and feel exactly like handwriting on a journal page. Once text is entered it persists in that same italic style, left-editable by clicking again.
5. **Switching Rooms** ("Change Room" button) opens a picker of the user's saved Rooms (named + thumbnailed); selecting one updates the Room photo across the mini Room card, the My Journeys panel, and the full Room screen everywhere simultaneously.
6. **The focus timer** (countdown shown both in the Current Chunk mini-card and full-size on the Room screen) is one shared, persistent timer tied to the "current chunk" (the next/active unchecked chunk). Pressing play starts the countdown; duration presets (15/20/30/45/60m) can reset/re-configure it; it is purely a focus/Pomodoro-style aid and does not auto-mark chunks complete.
7. **Profile name behavior:** by default, the top-right chip shows a generic placeholder (e.g. "My Profile"). If the user sets a name in Settings, that name (e.g. "Adhithya") replaces the placeholder everywhere the chip appears, alongside their uploaded avatar photo.
8. **Settings/Profile screen contains:**
   - Editable display name field and avatar upload.
   - **Rooms management:** add a new Room by uploading a background image and typing a name; list of existing Rooms with the ability to select/delete.
   - **Music management:** a list of default built-in ambient tracks plus any user-uploaded audio files (upload control to add new tracks); selecting one sets it as the active "No music playing"/now-playing pill at the top of the app.
9. **No calendar/date-picking UI exists anywhere in the app.** Dates shown (creation date, deadline date, the date stamp on Today) are all display-only/auto-generated, never an interactive scheduling control.
10. **Colors are consistent identifiers:** each Area owns one fixed color from the palette, and every Journey under it inherits/displays that same color as its accent (left bar, icon tile, dot) everywhere it's referenced across the app (Today metadata, Journeys list, My Journeys panel, sidebar accents are the one exception — sidebar tab stripes are fixed to red/yellow/blue/green per-tab regardless of Area colors).

---

## 7. VISUAL DESIGN SYSTEM

### 7.1 Overall Aesthetic
"Analog journal on a wooden desk." Warm, sepia, tactile, hand-finished — like a designer's physical planner photographed in soft window light. Nothing flat, glossy, or neon. Every surface has texture (paper grain, fiber, subtle stains/foxing at edges). Drop shadows are soft and warm-toned (never pure black) to suggest layered physical paper.

### 7.2 Color Palette

| Token | Approx. Hex | Usage |
|---|---|---|
| Paper base (light) | #E8DCC8 | Main page backgrounds, cards |
| Paper base (darker/sidebar) | #D8C7A8 | Sidebar background, secondary panels |
| Outer frame / desk wood | #1A1410 | Outermost app border |
| Ink black | #2B2420 | Primary text, titles |
| Ink red/maroon | #A33B2B / #8C2F22 | Stamps, underlines, checked checkboxes, "High" priority dot, CTA text, italic accent copy |
| Muted gray text | #7A6F5E | Secondary/meta text (e.g. "Journey: ... • Area: ...", dates) |
| Area — Career | #A33B2B (red) | Folder/icon/dot |
| Area — Home | #3B6FA0 (blue) | Folder/icon/dot |
| Area — Health | #4C7A4C (green) | Folder/icon/dot |
| Area — Personal | #C99A3A (yellow/gold) | Folder/icon/dot |
| Area — Creative | #7C5A9E (purple) | Folder/icon/dot |
| Area — Learning | #8A5A3B (brown/tan) | Folder/icon/dot |
| Priority — High | #A33B2B (red) | Dot + label |
| Priority — Medium | #D98C2B (orange) | Dot + label |
| Priority — Low | #3B6FA0 (blue) | Dot + label |
| Sticky note paper | #DDC9A0 | Affirmation/quote notes |
| Stamp ink (faded) | #9C4A3A, ~60–70% opacity, grainy | Date stamp, brand stamp, "FOCUS BUILD GROW" stamp |

### 7.3 Typography
- **Display/Headlines** ("Today", "Areas", "Journeys", Journey titles): a bold serif or slab-serif typewriter-adjacent face — sturdy, slightly vintage, high-contrast strokes. Think Georgia Bold / Tiempos Bold / a "typewriter headline" feel.
- **Body & UI text** (chunk titles, descriptions, list labels): a clean serif or semi-serif, smaller weight than headlines, very readable — same family as headlines but regular weight, or a companion humanist serif.
- **Italic accent text** (taglines, affirmations, "x chunks selected", note placeholders, sticky notes): the same serif family in italic, used specifically to feel "handwritten"/personal, always rendered in ink-red or soft gray, never black.
- **Numerals/stamps/timer countdowns**: a slightly condensed bold serif/slab numeral, rendered large and in red for the countdown timer and date stamp digits.
- **No sans-serif fonts anywhere** — the entire type system stays serif/typewriter to preserve the analog-journal feeling.

### 7.4 Iconography
- Simple, single-weight line icons (calendar, folder, clipboard/list, armchair, gear, question-mark, clock, paperclip, chevrons) — thin black/dark-brown stroke, no fill except where noted (checkboxes when checked, colored folder icons, colored journey-tile glyphs which are white-on-color).
- Journey-tile glyphs (briefcase, sofa, character, dumbbell, book) are simple flat white pictograms centered inside a solid rounded-square color tile.
- Folder icons for Areas are flat colored (not outline) with a small darker fold/tab detail at the top, like a manila folder.
- Paperclips and binder clips are rendered with light dimensionality/shading (metallic gray, subtle highlight) to look physically real, not flat-icon style — they're a textural/material detail, not a UI icon.

### 7.5 Textures & Materials
- **Paper:** visible fiber grain, soft mottled staining, very subtle vignette darkening at the page edges, deckled/torn top edges on the Today/Areas/Journeys main "pages."
- **Sidebar:** slightly darker, rougher kraft-paper-like texture compared to the central page, to visually recede behind it.
- **Stamps:** rendered as if rubber-stamped — uneven ink density, slight bleeding at character edges, faint texture showing through the ink.
- **Wood frame:** subtle wood-grain texture in near-black brown.
- **Binder clip & paperclips:** brushed-metal shading — dark gray body, a thin lighter highlight to suggest curvature.
- **Room photography:** warm, golden-hour lighting, shallow depth of field, photographic realism (not illustration) — this is the one part of the UI that's a real/rendered photo rather than flat illustrated UI.

### 7.6 Shape & Spacing Language
- Generous rounded corners on cards (~12–16px radius), but the main "pages" themselves have a mix of clean side edges and intentionally torn/irregular top edges.
- Hairline dividers (1px, low-opacity brown) separate list rows — never heavy borders.
- Comfortable padding throughout; nothing dense or cramped — the product should feel like flipping through a spacious, well-organized planner, not a cramped spreadsheet.
- Dashed-border outlines are reserved specifically for "add new ___" affordances (Add a chunk to today / Add new area / Add new journey / Add Journey) — this dashed style is the universal "create new" signal across the app.
- Colored accent bars (3–4px wide, full row height) on the left edge of list rows are used wherever an Area/Journey color needs to be flagged (My Journeys panel rows, sidebar tab stripes).

### 7.7 Motion/Interaction Feel (qualitative guidance)
- Transitions should feel like turning a page or sliding a folder forward — gentle, not snappy/bouncy.
- The right-column swap (My Journeys panel → Journey detail panel) should feel like pulling a folder out from a stack, complete with the binder-clip-and-peeking-tabs treatment.
- Checking a checkbox should give a small satisfying tick/strike animation (e.g., a quick hand-drawn checkmark draw-on plus a strike-through line animating left to right) — nothing flashy, no confetti/gamified celebration, keep it understated and calm.
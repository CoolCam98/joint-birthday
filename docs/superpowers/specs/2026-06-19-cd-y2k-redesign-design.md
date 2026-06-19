# CD / Y2K Redesign — Design

## Summary

Redesign the joint-birthday invite as a single static HTML page (no backend) with a CD-themed, Y2K/MySpace-scene visual identity. "CD" plays double duty as Camryn & Dallas's initials and an actual compact disc. The page is gated by a password on load; entering it reveals the full site (situation, logistics, a "What's Your Spirit Animal?" quiz, and RSVP). All existing copy is reused as-is, including the current event date — content accuracy is out of scope for this round.

## Visual theme

MySpace-scene / glitter CD aesthetic, confirmed via mockups:

- **Display font:** Comic Sans MS for headings, labels, buttons, badges. Body paragraphs stay in a clean sans-serif (system default / Inter) for readability.
- **Color system:** Vibrant CD-rainbow gradients (hot pink, cyan, yellow, purple) — each major section uses a different gradient from this palette so the page doesn't read as monotone. Dark semi-transparent cards (`rgba(0,0,0,0.4-0.55)`) sit on top of the gradients to hold body text and form fields with enough contrast to stay readable.
- **Borders:** Thick dashed borders (white or black depending on background) on cards, grid cells, and inputs — the recurring structural motif instead of the current site's thin solid hairlines.
- **Disc graphic:** A CSS-built CD — conic-gradient rainbow sweep, faint concentric ring texture (repeating-radial-gradient), a black/white center label ring, and a spindle hole — used on the lock screen and reused as a motif elsewhere (e.g. quiz, dividers).
- **Stickers:** Scattered emoji (💖 ✦ 💿 ⚡ 🎤 🎂) at varied rotation angles per section for visual energy, not confined to a grid.
- **Dividers:** Thick dashed repeating-gradient bars instead of the current thin solid divider.

**Y2K gimmicks included:**
- Glitter/sparkle text accents and scattered sparkle stickers
- Blinkies (small badge elements, e.g. "★ C+D 4EVER ★")
- A slow horizontal marquee ticker (~28s loop — confirmed slower than initial draft)
- A custom sparkle/CD-shaped cursor trail on mousemove (desktop only — naturally absent on touch devices, which is fine)

**Explicitly excluded:** retro visitor counter, "under construction" badge.

Respect `prefers-reduced-motion`: disable marquee animation, cursor trail, and unlock animation flourishes for users who request reduced motion (mirrors the existing site's handling of `.fade-in`).

## Page structure

Single HTML page with the following sections, in order:

1. **Lock screen** (always shown first, full-viewport)
2. *(on unlock)* Brief celebration beat — "★ you cracked the code ★"
3. **The Situation** (pink gradient section)
4. **The Logistics** — details grid + Terms & Conditions fine print (cyan gradient section)
5. **What's Your Spirit Animal?** quiz (yellow gradient section)
6. **RSVP** (purple gradient section)
7. **Footer** (existing copy, unchanged)

There is no separate "save the date" page with partial info — the password gates the entire page. Nothing about the event is visible until the correct password is entered.

## Lock screen

Full-viewport, hot pink/purple gradient (matching the approved mockup), containing in order:
- Slow marquee ticker across the top: "✨ you are cordially invited... but you gotta know the magic word ✨"
- CD disc graphic
- Names: "camryn & dallas" (Comic Sans, rotated slightly, black text-shadow outline)
- Existing tagline copy, verbatim: "A joint birthday celebration. *Not weird. It's economical.*"
- Blinkie: "★ C+D 4EVER ★"
- Password input + "UNLOCK ✦" button

**Password mechanics:** Plain client-side JS comparison against the fixed word `mixtape` (case-insensitive, trimmed). This is a fun gate for a party invite, not real security — acceptable since the password is intentionally shared with invitees out-of-band (text/save-the-date card), and the only thing it protects is party logistics, not sensitive data.

**Unlock behavior:** On correct entry, play a short animation (CD spin + glitter burst) on the lock screen, then it fades out to reveal the rest of the page, which opens with the "you cracked the code" celebration line before continuing into The Situation. Wrong password: shake the input and show an inline error (no specific hint text beyond what's already on screen).

## The Situation / The Logistics

Direct visual reskin of the existing content — same copy, same data (date, time, venue, location, format, fine print) — restyled into the gradient-section / dashed-card / Comic Sans system described above. Details grid collapses to a single column on mobile (kept from the original site's existing breakpoint behavior, just restyled).

## Quiz: "What's Your Spirit Animal?"

Standalone fun section, not tied into the RSVP form or its data. Fully interactive: 5 multiple-choice questions, each with 4 options; selecting an option advances to the next question (or to the result on the last question). A point-tally scoring system assigns each answer option 1-2 points toward one or more of 7 possible results; whichever result accumulates the most points wins, with ties broken by picking randomly among the tied results (keeps it feeling fun/unpredictable rather than rigidly deterministic). Include a "retake quiz" action on the result screen.

### Questions & scoring

Result keys: `raccoon`, `jellyfish`, `retriever`, `pigeon`, `capybara`, `chihuahua`, `crow`

**Q1. Karaoke is starting. What's your move?**
- A) Belt a power ballad like it's the Grammys, tears optional but likely → `retriever`, `jellyfish`
- B) Pick something so obscure no one can sing along, on purpose → `pigeon`, `crow`
- C) Hijack the mic for a duet nobody agreed to → `chihuahua`, `raccoon`
- D) Quietly slip away from the mic and reappear with a fresh drink → `capybara`, `pigeon`

**Q2. Someone asks "wait, whose idea was this combined party?" You say:**
- A) "Mine, obviously. Have you met us?" → `jellyfish`, `chihuahua`
- B) "Does it matter? There's free karaoke." → `capybara`
- C) "Honestly? Chaos. Pure chaos." → `raccoon`
- D) You're already gone, no one knows where → `pigeon`, `crow`

**Q3. It's 11:47pm. Where are you?**
- A) Still on the mic, two songs deep into a setlist no one asked for → `chihuahua`, `jellyfish`
- B) In the corner, watching everything happen, saying nothing → `pigeon`, `crow`
- C) Outside "getting air" (you are fine) (you are lying) → `retriever`
- D) Floating through the night vibes-first, no plan, no worries → `capybara`, `raccoon`

**Q4. Your honest plus-one strategy:**
- A) Bring someone unhinged to match your energy → `raccoon`, `chihuahua`
- B) Bring someone calm to balance you out → `capybara`
- C) Didn't tell them where we're going, surprise → `crow`, `pigeon`
- D) You ARE the plus one's plus one, it's complicated → `retriever`, `pigeon`

**Q5. A CD/mixtape is made about your night. What's track 1?**
- A) Something loud, dramatic, way too much build-up → `jellyfish`, `chihuahua`
- B) A deep cut nobody's heard, but it's perfect → `pigeon`, `crow`
- C) A feel-good song you'll play 4 more times → `retriever`, `capybara`
- D) Whatever's already playing, you didn't pick it → `raccoon`, `capybara`

### Results

| Key | Title | Emoji | Description |
|---|---|---|---|
| `raccoon` | A Raccoon That Fell In The Pool But Is Completely Unbothered | 🦝 | You thrive in chaos, you will always land on your feet, and honestly? You looked great doing it. No notes. |
| `jellyfish` | A Disco Ball Jellyfish | 🪩 | You glow without trying, you're a little too much in the best way, and you will absolutely end up the main character of someone's Instagram story tonight. |
| `retriever` | A Golden Retriever That Just Saw A Ghost | 🐕 | You love everyone immediately and full-body flinch at literally everything. A beautiful, anxious mess. We adore you. |
| `pigeon` | A Pigeon That Has Personally Seen Too Much | 🐦 | You've seen things. You don't talk about it. You will still show up, observe everything, and somehow know all the gossip by midnight. |
| `capybara` | A Capybara Floating Down A Lazy River With A Margarita | 🦦 | Nothing fazes you. You are simply floating. The party could catch fire and you'd ask if anyone wants to split a quesadilla first. |
| `chihuahua` | A Chihuahua In A Sweater Barking At The Vacuum | 🐶 | Small, loud, and absolutely ready to fight something twice your size for no reason. We respect the commitment. |
| `crow` | A Crow That Stole Someone's Car Keys And Is Thriving | 🐦‍⬛ | You are clever, a little bit feral, and currently in possession of something that is not yours. Iconic behavior, never change. |

## RSVP

Same four questions as the existing form, restyled into the theme, on the purple gradient section:
1. Name (text input)
2. Will you be attending? (yes/no radio, existing copy)
3. Bringing a plus one? (yes/no radio, existing copy)
4. "While We Have You" — future event communications (3-way radio: yes / maybe / no, existing copy)

**Submission:** Wired to **Netlify Forms** (the site already deploys via Netlify). Implementation needs:
- A static hidden form in the HTML with `data-netlify="true"`, `name="rsvp"`, and a field for every input in the visible form — required so Netlify's build-time bot can detect and register the form (Netlify Forms only picks up forms it can find at build time, not ones rendered purely by JS).
- The visible, styled form submits via `fetch()` with `Content-Type: application/x-www-form-urlencoded`, including a `form-name=rsvp` field, POSTed to `/` — this avoids a page navigation/redirect.
- On successful submission, show the existing in-page "You're In" success state (same UX as today, just now backed by a real submission instead of a no-op).
- On fetch failure, show an inline error and leave the form intact so the guest can retry.

No separate backend, no third-party form service account needed — this rides on the existing Netlify deploy.

## Mobile

Mobile is the primary target. Layout and type sizes are designed mobile-first (validated at 390px width in mockups) and scale up for desktop, rather than the reverse:
- Single-column layouts throughout (details grid, quiz options, RSVP fields) by default, no desktop-only multi-column requirement.
- Large tap targets (buttons/inputs ≥ 44px touch target height).
- `clamp()`-based responsive type sizing, consistent with the current site's approach.
- Cursor-trail effect only attaches `mousemove` listeners — naturally inert on touch devices.

## Out of scope

- Updating the event date/time/details for accuracy (explicitly deferred — copy is reused as-is)
- Any quiz-to-RSVP data linkage
- Real authentication/security on the password gate

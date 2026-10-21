# Look: bragi (Justin Harris, "quiet operator" editorial)

The only look Bragi uses for talking reels. It starts from the stock **cool dude** look (dark editorial, tracked caps
plus big serif words, text behind the head) and swaps every colour, font and finish for JHAI brand law. Read
`rules/bragi/RULES.md` first: where this file and RULES.md disagree, RULES.md wins.

Target, measured on the top AI-creator reels in `research/bragi-video-edit-2026-10/references/spec.md` (Chase AI, Nate
Herk, Jack Roberts): visual change every 1.5 to 3 s, 3 to 6 cuts or punches per 10 s, 2 to 4 word caption chunks
changing every 0.3 to 0.5 s, a headline on frame 1, proof cards that pop in over 6 to 10 frames and hold 0.5 to 1.5 s.

## Build machinery
Copy `SK/assets/template/` as usual, then copy `SK/looks/bragi/fonts/` into the project's `assets/fonts/` and use
`looks/bragi/fonts/fonts.css` instead of the template's font list. Keep the template's machinery (cut times from
segments.json, `S(t)`, cutout layer order, SFX lanes, safe guide). Delete the template's YEL, cream, teal tone, warm
overlay, vignette, light sweep, grain and text-shadow code paths; they are all off-brand. `dude_words()` and `cards()`
are the closest starting points.

## Footage
- Natural colour. CSS filter on the footage layers at most `contrast(1.03) saturate(1.0)`; a small `brightness()`
  lift only if the clip is underexposed. Base video and cutout get the same filter so the subject matches the room.
- No overlays on top of the footage except the flat elements below.

## Hook (first 2 seconds)
- Open on the strongest complete sentence in the clip (reorder if needed; snap to a sentence start).
- Frame 1 already has type on it: a Geist Mono kicker (2 to 4 words, e.g. `AI ADVICE · 01`) plus a big Piazzolla
  headline of 2 to 5 words that states the claim, with its key word in Piazzolla Italic 600 Glow. Words land on
  their spoken frames; the headline may lead the voice by up to 0.3 s on frame 1.
- A punch-in (scale 1.0 to 1.12, 0.25 s, power3.out) on frame 1 so the very first frame moves.

## Big words (the signature)
- 2 to 6 big lines per 60 s, on the lines that carry the argument. Piazzolla Regular 400, 150 to 230 px, line height
  1.0, letter spacing -0.03em, white. The single key word per line: Piazzolla Italic 600, same size, Glow `#A6F0DA`.
- Placement: tucked BEHIND the top of the head (cutout layer above the type) using the layout.md formula, or over
  the chest band in front when the head moves. Never across the eyes, nose or mouth.
- Entrance: per-word mask rise (`yPercent 110 -> 0`, 0.38 s, expo.out, stagger 0.05) with a 0.2 s blur 8px -> 0
  settle; the key word lands last with a 1.06 -> 1 scale. Exit: 0.14 s opacity out, then `tl.set` kill.
- A Geist Mono kicker sits 24 px above the first line: 30 px, uppercase, letter-spacing 0.14em, white 70%,
  tracks in with `scaleX 1.3 -> 1` over 0.3 s (never tween letterSpacing).
- While a big line holds Glow, captions show no Glow (one Glow element per frame).

## Captions: preset E "Quiet Operator", every spoken word
- Geist SemiBold 600, 58 px, sentence case, white. Chunks of 2 to 4 words grouped by meaning (never more than 4,
  never split a word, break at punctuation first).
- Flat Ink `#161B1C` band at 78% opacity behind each chunk: 24 px top and bottom, 39 px left and right, 13 px
  corners, width hugs the text, centred, band top at y about 1306 (68%). Move it between y 1150 and 1400 only to
  stay off the face and inside the safe zone.
- Spoken state: unsaid words 62% white, said words 100% (tl.set on each word's start frame).
- Chunk entrance: band scales 0.96 -> 1 and fades in over 0.12 s (power2.out); words do not bounce.
- Key words (numbers, names, the claim word) may switch to Piazzolla Italic 600 at 71 px in Glow, only when no big
  line or card is showing Glow at that moment.
- Hide captions while a full proof card covers the caption band; never stack a caption on a card.

## Punch-ins and cut rhythm
- Every segment change is a cut. Inside a take longer than 3 s, add a hard punch every 2 to 3 s, cycling zoom
  1.0 -> 1.28 -> 1.0 -> 1.5 (crop from the 4K original, cx/cy on the face so eyes stay in the upper third).
- Hard punch = instant on the frame plus a 0.18 s settle from 1.03x of the new zoom. No crossfades.
- The zoom LEVEL must really change: consecutive segments never share a zoom. A settle from 1.03x alone is not a
  punch (judge round 1 scored that "locked composition", motion 5/10).
- Between punches the frame is never static: a slow drift on `#stage` of about 3% scale plus 10 to 20 px of x/y
  over each segment (sine.inOut), direction alternating per segment.
- Target 5 to 7 visual changes per 10 s counting cuts, punches, big lines, cards and full-screen beats.

## Motion density (judge round 1: motion 5.0 vs references 8 to 9)
The references move something on screen almost every second. Bragi must too, without breaking brand:
- **Full-screen beats:** 2 or 3 moments per 30 s where a flat Ink `#161B1C` full-frame cutaway takes over for 0.7 to
  1.2 s on the line that carries the argument (the headline claim, a number, the closing line). Giant Piazzolla type
  (220 to 320 px) builds word by word on the spoken frames; the key word lands in Italic 600 Glow. Cut in and out
  hard on word boundaries (audio continues underneath). These replace the face only briefly; never two in a row.
- **Numbers count:** a spoken number animates (count up or roll per digit over 0.4 s) instead of appearing.
- **Hook:** frame 1 shows the headline BIG (Piazzolla 150 px or more) over the top zone above the eyes (hat, sky,
  wall), not a small boxed label. The punch-in on frame 1 goes to at least 1.15x.
- **Captions breathe:** each word, as it is said, pops 1.08 to 1 over 0.12 s along with the 62 to 100% white step.
- **Lists build:** each list row slides in, and the previous row's tick or rule travels to the new row (one Glow
  element moving, not two).
- **Exits move too:** big lines and cards leave with a 0.18 s rise plus fade, never a plain opacity cut.
- **Sound follows motion:** a soft whoosh on every full-screen beat and big line, a tick on each list row.

## Illustrate the sentence (tenfoldmarc's method, adopted)
Before building, read the transcript and mark 4 to 8 beats worth illustrating: a comparison, a number, a claim, a
tool name, a list, the call to action. Each beat gets exactly ONE graphic, timed to the exact word, drawn in HTML/CSS:
a comparison is a versus card, a number is giant Piazzolla type with a Geist Mono unit, a list is a checklist card,
a prompt is a typing terminal, "follow" is a follow button that clicks itself. Leave some beats with just Justin and
the captions. Graphics live in the empty zone (usually the top zone, y 220 to 520, above the head, or below the chin)
and never cover the face. Entrance about 0.3 s (scale 0.9 to 1, 30 px rise, back.out), exit about 0.2 s fade with a
slight rise. Copy the method, never Marc's graphics.

## Proof and list cards
- When Justin lists things ("they don't have X, they don't have Y"), build ONE flat Ink card (`#161B1C`, 8 px
  corners, no border, no shadow) under the chin: a Geist Mono pill label at top (`THE BASICS`), then one row per
  item in Geist SemiBold 44 px white, each row appearing on its spoken word with a 0.22 s slide-up of 24 px; the row
  being spoken gets a single Glow tick or rule (the frame's one Glow element), finished rows drop to 70% white.
- Cards pop in with scale 0.92 -> 1 plus opacity over 8 frames (back.out(1.4)), hold, exit 6 frames.
- Cards stay inside the safe zone (bottom edge y <= 1470) and below the chin (headpos.py).

## Sound
- Use the template's SFX at its subtle gains: a soft whoosh on each big line entrance, a soft tick on each card row,
  a pop on the card entrance. Voice always louder. Loudness -14 LUFS, true peak -1.5 dB.
- No music unless a royalty-free track is already in the project.

## End
- Last spoken line ends on a big line or the caption. Then 0.8 s flat Ink end card: white lockup from
  `looks/bragi/logo/jh-lockup-white.svg` at 360 px wide centred, Geist Mono `JUSTINHARRIS.AI` 26 px white 70%
  below it. Fade in 0.2 s. Nothing in Glow on the end card.

## QA before render (in addition to the skill's own)
- Snapshot 12 to 20 moments with `--safe` and check: one Glow element at most per frame, no text on the face, no
  yellow/cream/teal/black (`#000`) anywhere in index.html (`grep -iE "FAE67A|F7F4EE|FFF8EF|#000\b|text-shadow|
  gradient"` must be empty except the safe-guide), no em or en dashes in any on-screen string.

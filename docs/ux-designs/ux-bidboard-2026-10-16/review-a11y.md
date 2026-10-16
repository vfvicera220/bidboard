# BidBoard UX spec — Accessibility review (WCAG 2.2 AA)

- **Reviewed:** `DESIGN.md`, `EXPERIENCE.md`, `mockups/buyer-bidding.html` (Round 3 overrides applied), `mockups/seller-screens.html`
- **Date:** 2026-10-16
- **Totals:** 1 blocker · 10 major · 12 minor

## 1. Contrast — measured

The ratios below were computed with the WCAG 2.x relative-luminance formula, using hex tokens parsed directly from the `DESIGN.md` frontmatter. I ran an inline Python script that parsed the frontmatter, so no extra file was added to the workspace. Thresholds: 4.5:1 for text, including 11–13px bold, because none of it qualifies as large text. 3:1 for non-text UI and focus indicators (1.4.3, 1.4.11). Alpha scrims are excluded.

### Light

| Result | Ratio | Foreground on background | Where used |
|---|---|---|---|
| PASS | 16.24 | ink #18181B on background #F5F5F4 | text on page / selected row |
| PASS | 17.72 | ink #18181B on surface #FFFFFF | body text |
| PASS | 7.73 | ink-secondary #52525B on surface #FFFFFF | labels, meta, "when" |
| PASS | 7.09 | ink-secondary #52525B on background #F5F5F4 | trust signal, selected-row meta |
| PASS | 17.72 | on-primary #FFFFFF on primary #18181B | Bid / primary buttons, selected increment chip |
| PASS | 14.63 | ink on success-tint #DEEDE4 | Winning banner, Paid chip |
| PASS | 4.72 | success-label #137638 on success-tint #DEEDE4 | Winning banner label |
| PASS | 15.91 | ink on warning-tint #FEF3C7 | Extended banner, Awaiting chip |
| PASS | 14.50 | ink on danger-tint #FEE2E2 | Outbid banner, Unpaid chip |
| PASS | 15.84 | ink on accent-tint #EEF2FF | Open banner, Open chip |
| PASS | 4.83 | on-live #FFFFFF on live #DC2626 | LIVE pill |
| PASS | 4.83 | live #DC2626 on surface | countdown, "Live" status |
| **FAIL** | **4.43** | **live #DC2626 on background #F5F5F4** | **"Live" status on the selected control-room row (`item-row-selected`, laptop)** |
| PASS | 5.02 | success #15803D on surface | ▲ tick, "You lead" |
| PASS | 4.60 | success #15803D on background | ▲ tick on selected row (thin margin) |
| PASS | 6.47 | danger #B91C1C on surface | Outbid text, field error, destructive label |
| PASS | 5.93 | danger #B91C1C on background | Outbid on selected row |
| PASS | 4.92 | warning #A16207 on surface | warning status text |
| PASS | 6.29 | accent #4F46E5 on surface | ● OPEN |
| PASS | 5.76 | accent #4F46E5 on background | Open on selected row |
| PASS | 5.86 | ink-secondary on photo-placeholder #E0E0E1 | photo slot label |
| PASS | 6.09 | ink-secondary on border #E4E4E7 | segmented control unselected label |
| **FAIL** | **1.27** | **border #E4E4E7 vs surface #FFFFFF** | **input, option-card, increment-chip, quiet-button boundary (1.4.11)** |
| **FAIL** | **1.16** | **border #E4E4E7 vs background #F5F5F4** | **stepper button frame** |
| FAIL (mitigated) | 1.09 | background #F5F5F4 vs surface | stepper fill vs action bar (the ink glyph identifies it) |
| **FAIL** | **1.27** | **surface knob/segment vs border track** | **toggle OFF state; selected segment vs track** |
| PASS | 17.72 | ink vs surface | bid-amount frame, secondary/Buyout outline, toggle ON, option selected |
| PASS | 7.73 | ink-secondary vs surface | radio circle, dashed "add photo" |
| PASS | 6.29 / 5.76 | focus #4F46E5 vs surface / background | focus ring (2px offset) |
| PASS | 5.15–5.65 | focus vs each tint | focus ring inside banners and chips |
| FAIL (conditional) | 2.82 | focus #4F46E5 vs primary/ink #18181B | ring with no offset gap against the Bid button, ink frame or action-bar rule |
| PASS | 4.14 / 4.42 / 5.30 / 5.62 | success / warning / danger / accent vs own tint | chip border, dot, banner rule |
| PASS | 5.02 / 4.92 / 4.83 | success / warning / live vs surface | leader edge, flash edge, LIVE pill edge |

### Dark

| Result | Ratio | Foreground on background | Where used |
|---|---|---|---|
| PASS | 19.06 | ink-dark #FAFAFA on background-dark #09090B | text on page |
| PASS | 16.97 | ink-dark on surface-dark #18181B | body text (DESIGN.md claims 17.4) |
| PASS | 6.91 | ink-secondary-dark #A1A1AA on surface-dark | labels, meta (DESIGN.md claims 7.8) |
| PASS | 7.76 | ink-secondary-dark on background-dark | trust signal |
| PASS | 19.06 | on-primary-dark #09090B on primary-dark #FAFAFA | Bid / primary |
| PASS | 12.72 / 14.81 / 16.47 / 15.80 | ink-dark on success / warning / danger / accent tint | banners, chips |
| PASS | 7.62 | success-label-dark #4ADE80 on success-tint-dark #1F3429 | Winning label |
| PASS | 7.19 | on-live-dark #09090B on live-dark #F87171 | LIVE pill |
| PASS | 6.40 / 7.19 | live-dark on surface / background | countdown, Live |
| PASS | 10.17 / 11.42 | success-dark on surface / background | ▲, You lead |
| PASS | 6.40 / 7.19 | danger-dark on surface / background | Outbid, errors |
| PASS | 11.57 | warning-dark on surface | warning text |
| PASS | 5.94 / 6.67 | accent-dark on surface / background | ● OPEN |
| PASS | 5.05 | ink-secondary-dark on photo-placeholder-dark #313135 | photo slot |
| PASS | 5.27 | ink-secondary-dark on border-dark #2E2E33 | segment label |
| **FAIL** | **1.31** | **border-dark #2E2E33 vs surface-dark #18181B** | **input, option-card, chip, quiet-button boundary** |
| **FAIL** | **1.47** | **border-dark vs background-dark** | **stepper frame** |
| FAIL (mitigated) | 1.12 | background-dark vs surface-dark | stepper fill |
| **FAIL** | **1.31** | **surface-dark knob/segment vs border-dark track** | **toggle OFF; selected segment** |
| PASS | 16.97 | ink-dark vs surface-dark | outlines, frames, toggle ON |
| PASS | 5.94 / 6.67 | focus-dark #818CF8 vs surface / background | focus ring |
| PASS | 4.45–5.76 | focus-dark vs each dark tint | focus in banners |
| FAIL (conditional) | 2.86 | focus-dark #818CF8 vs primary-dark #FAFAFA | ring with no offset against a dark-theme Bid button or ink frame |
| PASS | 7.62 / 10.10 / 6.21 / 5.53 | status vs own dark tint | chip border, dot, rule |

The 1px hairline row dividers and stats-grid gaps are decorative, so they're exempt from 1.4.11. They are not listed as failures.

### Suggested replacements (all measured)

| Token | Current | Replacement | Result |
|---|---|---|---|
| `live` (light) | #DC2626 | **#D02222** (minimum) or **#C81E1E** (headroom) | #D02222: 4.89 on background, 5.34 on surface, white-on-fill 5.34. #C81E1E: 5.26 / 5.74 / 5.74 |
| new `border-strong` (light) for input, option card, unselected chip, toggle-off track, stepper frame | #E4E4E7 | **#8A8A93** | 3.42 vs surface, 3.14 vs background |
| new `border-strong-dark` | #2E2E33 | **#6A6A72** | 3.30 vs surface-dark, 3.71 vs background-dark |
| `focus` (light), optional | #4F46E5 | **#6366F1** | 4.47 surface, 4.09 background, 3.97 vs ink, 3.66–4.01 vs light tints. Passes in every light adjacency, with or without the offset |
| `focus-dark` | #818CF8 | keep, but **mandate the 2px offset** (or a 1px surface inner ring) | 2.86 vs #FAFAFA only when the ring touches the fill. Do not use #6366F1 in dark: it's 2.97 on success-tint-dark |

Keep `border` / `border-dark` for decorative hairlines only.

## 2. Findings

### Blocker

**B1 · 4.1.2 Name, Role, Value; 1.3.1 Info and Relationships · EXPERIENCE.md › Foundation / Responsive & Platform ("One Flutter codebase ships as mobile web")**
- **Finding:** Flutter web builds its semantics (DOM accessibility) tree lazily. By default a screen-reader user has to find and activate a hidden "Enable accessibility" placeholder before any button, label or live region exists. Buyers arrive via Flutter web from a Facebook link, so the whole primary bidding flow can be unusable with VoiceOver or TalkBack. The spec's WCAG 2.2 AA claim for mobile web depends on this, and it isn't mentioned.
- **Fix:** Add a platform requirement to call `SemanticsBinding.instance.ensureSemantics()` at app start on web. Every interactive widget needs an explicit `Semantics` label, role and state. Add a release gate: test the bid flow with VoiceOver in iOS Safari, the Facebook in-app browser, and TalkBack in Chrome.

### Major

**M1 · 1.4.11 Non-text Contrast · DESIGN.md › components.input, option-card, increment-chip, button-quiet, bid-stepper-button**
- **Finding:** The boundaries of these controls use `{colors.border}`: 1.27:1 light, 1.31:1 dark (1.16 / 1.47 on background for the stepper). A text input has no other visual cue, so an empty field is effectively invisible.
- **Fix:** Add a `border-strong` token (#8A8A93 light / #6A6A72 dark) and use it for input, option-card (unselected), increment-chip (unselected) and the stepper frame. Quiet buttons can keep the hairline because their label identifies them, but use `border-strong` for consistency.

**M2 · 1.4.11 Non-text Contrast; 1.4.1 Use of Color · DESIGN.md › components.toggle, segmented-control**
- **Finding:** Toggle OFF is a surface knob on a `border` track: 1.27 / 1.31. The selected segment is surface on a `border` track (1.27 / 1.31), plus a 0.12-alpha shadow and a text change from ink-secondary to ink. In practice the state shows only through the text colour and weight change.
- **Fix:**
  - Toggle-off track: `border-strong`, or ink-secondary #52525B / #A1A1AA, with the knob outlined.
  - Selected segment: add a non-colour indicator, such as a 2px ink underline or outline (ink is 17.7 / 17.0).
  - Expose state: toggle `role=switch` with checked state; segments as a tablist/radiogroup with selected state.

**M3 · 1.4.3 Contrast (Minimum) · DESIGN.md › item-row-selected; seller-screens.html laptop control room `.cr.flash.sel` "Live"**
- **Finding:** Status text in `{colors.live}` #DC2626 on the selected-row ground `{colors.background}` #F5F5F4 measures **4.43:1** at 11px/700. EXPERIENCE's "all text pairs ≥4.5:1" is therefore false.
- **Fix:** Change `live` to **#D02222** (4.89 / 5.34, white-on-fill 5.34) or #C81E1E (5.26 / 5.74). Alternatively, render the selected row on `surface` with the ink edge only. Separately, success on background is only 4.60, so leave no room for further lightening.

**M4 · 2.2.1 Timing Adjustable; 2.2.2 Pause, Stop, Hide · EXPERIENCE.md › Interaction Primitives (Anti-snipe), Accessibility Floor, OQ-2, OQ-10**
- **Finding:** The spec never states that live auction close is the real-time essential exception to 2.2.1 and 2.2.2, and it gives no warnings. Countdown thresholds are left open (OQ-10). "Offer to next bidder" may get a time limit (OQ-2), and that isn't a real-time event, so the exception wouldn't cover it. The auto-updating ladder (newest first) shifts rows under a screen-reader user's reading position, with no pause.
- **Fix:** Add a "Timing" bullet to the Accessibility Floor:
  1. Cite the 2.2.1 real-time exception for timed auctions, and that open-ended items have no limit.
  2. Give announced warnings at 5:00, 1:00, 0:30 and 0:10, then "Bidding closed". Announce each anti-snipe extension once to every viewer ("Closing extended, 30 seconds left").
  3. Any Offer-to-next-bidder deadline must be ≥20 hours, or extendable.
  4. Add a "Pause live ladder" control that freezes the ladder and stats view while price, clock and action bar stay live, with "N new bids" to resume.
  5. Close OQ-10 with these thresholds.

**M5 · 4.1.3 Status Messages · EXPERIENCE.md › Accessibility Floor (live regions)**
- **Finding:** The outbid (assertive) and other people's bids (polite, coalesced) rules are good but incomplete. Not covered:
  - bid placed or confirmed
  - bid rejected or overtaken
  - stepper reset to a new next bid, which silently changes the value the user is about to commit
  - auction closed (won, lost or sold)
  - Reconnecting / Reconnected, and Bid disabled
  - Buyout success
  - control room "closed early"
  - blocked-bid outcome
- **Fix:** Add an announcement table:

  | Event | Priority | Announcement |
  |---|---|---|
  | Bid placed | polite | "Bid ₱4,600 placed. You're winning." |
  | Bid rejected | assertive | "Your bid wasn't placed. Price is now ₱4,700. Your amount reset to ₱4,800." |
  | Stepper reset with no bid attempt | polite | "Next bid changed to ₱4,800." |
  | Close | assertive | "Bidding closed. You won at ₱4,900." / "Sold to another bidder at ₱…" |
  | Reconnecting / Reconnected | polite, once each | "Reconnecting. Bidding paused." / "Reconnected." |

  None of these may move focus. In Flutter, use `SemanticsService.announce(..., assertiveness:)` or `Semantics(liveRegion: true)`.

**M6 · 2.4.11 Focus Not Obscured (Minimum) · DESIGN.md › bid-action-bar ("fixed to viewport bottom"); seller tab-bar**
- **Finding:** Keyboard or switch users tabbing through the ladder, ⋯ buttons or the batch rows will land on items hidden behind the fixed two-row action bar (about 120px, more at 200% text) or the tab bar. Nothing in the spec prevents this.
- **Fix:** Require bottom scroll padding equal to the bar height (`scroll-padding-bottom`, or `Scrollable.ensureVisible` with an alignment policy in Flutter). Any element receiving focus must be scrolled fully above the bar. Bottom sheets must not leave the trigger's focus hidden after close.

**M7 · 2.4.7 Focus Visible; 1.4.11 · DESIGN.md › components.focus-ring; full-width strips; `overflow:hidden` banners and grids; both mockups**
- **Finding:** The ring is an outer outline with a 2px offset. On edge-to-edge strips (item rows, batch lot rows, control-room rows, the ladder, the action-bar container) the outer ring falls off-screen or gets clipped by parent clipping (`overflow:hidden` on `.ph`, `.ban`, `.grid`, and Flutter `ClipRRect`). Neither mockup shows a focus state at all.
- **Fix:** Specify an inset ring (3px focus, drawn inside, 2px surface gap) for full-width rows and clipped containers. Add a focused-state example to the mockups. Where the ring touches an ink or primary fill with no gap (2.82 light, 2.86 dark), keep the gap mandatory, or switch light `focus` to #6366F1.

**M8 · 3.3.8 Accessible Authentication (Minimum) · EXPERIENCE.md › Foundation ("Sign in with Facebook only"), Sign-in sheet (not mocked)**
- **Finding:** The only route is a third-party OAuth flow, and it isn't specified. 3.3.8 holds only if Facebook's own step allows password managers and paste, and BidBoard adds no cognitive test. Risks:
  - an embedded WebView, especially the Facebook in-app browser, that blocks autofill or password managers
  - a BidBoard-side CAPTCHA or bot check
  - forced re-auth during the final seconds of an auction
- **Fix:** In the Sign-in sheet spec:
  1. Use the platform auth session (`ASWebAuthenticationSession` / Custom Tabs, or a full redirect on web), never an embedded WebView.
  2. No app-side CAPTCHA or puzzle.
  3. Keep the session long enough that sign-in isn't re-prompted mid-auction.
  4. Preserve the selected amount (already specified).
  5. The sheet title and button have accessible names, and errors are described in text.

  Note that Google sign-in is deferred, so there's no alternative route.

**M9 · 1.4.4 Resize Text; 1.4.10 Reflow · DESIGN.md › item-row columns `1fr 82px 36px 58px`, stats-grid 4 columns, bid-action-bar fixed; EXPERIENCE.md › Text scaling**
- **Finding:** "Must not truncate at 200%" is stated, but the layout fights it:
  - The control-room columns are fixed px.
  - The 4-up stats grid at 390px has about 85px cells holding "₱8,000" mono at 26px.
  - The fixed two-row bar at 200% (or in landscape) can take more than half the viewport, so the ladder can't be reached.
  - `pill` lineHeight 1 clips glyphs when scaled.
  - On Flutter web, the iOS text-size setting doesn't reach the canvas, so only browser zoom applies.
- **Fix:**
  - Use flexible (fr/intrinsic) columns that stack status below the bid when the text scale is above 1.3.
  - Collapse the stats grid to 2×2 at a text scale ≥1.5.
  - When the scale is ≥1.5 or the viewport is under 500px high, make the action bar a single row, or non-fixed (sticky at the end of content), plus a "Bid" jump.
  - Set `pill` line-height to 1.2 or more.
  - Respect `MediaQuery.textScaler` without clamping, and verify 200% browser zoom on Flutter web.

**M10 · 4.1.2 Name, Role, Value; 3.3.1 Error Identification · EXPERIENCE.md › Bid stepper / Custom amount; buyer-bidding.html `.act`**
- **Finding:**
  - The labels "Raise bid by ₱100" and "Lower bid by ₱100" are good, but the spec doesn't say the amount has a role, a value or a name. A screen-reader user pressing + hears nothing about the new value.
  - The disabled "−" at the floor has no specified semantics.
  - The custom-amount inline error isn't programmatically tied to the field.
  - In the mockup, `aria-label` sits on bare `<div>`s with no role, so it's ignored.
- **Fix:**
  - Make the amount a labelled field, "Your bid, ₱4,600", exposed as an editable text field (or `Semantics(value:, increasedValue:, decreasedValue:, onIncrease/onDecrease)` as an adjustable).
  - After +/−, announce the new amount politely and update the Bid button's name.
  - Disabled "−" reads "Lower bid, unavailable, ₱4,600 is the lowest valid bid".
  - Link the error with `aria-describedby` or Semantics hint, and announce it on change.
  - Bid and Buyout are `button`s whose names include the amount.

### Minor

**m1 · 1.3.1 / 2.4.6 · EXPERIENCE.md › Voice ("J***", "Marco R."), OQ-17; ladder rows.** Screen readers read "J***" as "J star star star" or skip the asterisks. Give masked names a semantic label ("Bidder J, name hidden") and keep the visual mask. Settle one shortening rule (OQ-17) so the labels are predictable. The buyer's own row should read "You (Marco R.)".

**m2 · 1.3.1 / 3.1.4 (advisory) · ladder "When" column, stats labels, trust signal, countdown.** "1m" is read as "1 metre", "90d", "Incr" and "02:14" read badly. Add semantic labels: "1 minute ago", "Increment ₱100", "2 unpaid wins in the last 90 days", "2 minutes 14 seconds left". The countdown's semantic label must not update every second (see M4).

**m3 · 4.1.3 · Accessibility Floor ("coalesced").** No coalescing window is given. Specify at most one polite price announcement per 10 seconds, the latest only. Suppress them while the user is editing the amount or a sheet is open. Add a buyer setting: "Announce: all price changes / my status only / off". Outbid always stays assertive.

**m4 · 2.3.1 Three Flashes; 2.3.3 (advisory) · motion.flash-duration, item-row-flash.** In a bidding war the 120ms flash can repeat more than 3 times a second across control-room rows. Rate-limit flashes per element to at most 1 per second, and skip flashes when bursts are coalesced. Name the reduced-motion signal explicitly: `MediaQuery.disableAnimations` / `accessibleNavigation` in Flutter and `prefers-reduced-motion` on web. The ▲ text persists (already specified).

**m5 · 2.4.3 Focus Order; 2.4.1 Bypass Blocks · item page, control room.** No focus order is defined, and the action bar comes last in the DOM after the full ladder. Specify the order:
1. header
2. title
3. clock
4. banner
5. price
6. stats
7. ladder (with ⋯ buttons)
8. action bar (−, amount, +, Bid, Buyout)

Add a "Skip to bid" control after the title. A newly appearing banner must not take focus. On laptop, selecting a row keeps focus on the row and announces "Showing ladder for Seiko 5" politely.

**m6 · 1.4.11 · focus ring vs ink/primary (2.82 light / 2.86 dark without offset).** See replacements: light `focus` #6366F1 passes every light adjacency. In dark, keep #818CF8 and require the offset gap.

**m7 · 1.4.1 · tab-bar active state, segmented control.** The active tab is shown only by ink vs ink-secondary colour plus bold. Add a 2px ink indicator bar, and expose the selected state (`aria-current` / Semantics `selected`).

**m8 · 2.5.7 Dragging Movements · photo-slot ("first is the cover").** How photos are reordered or the cover chosen isn't specified. If it's drag-to-reorder, provide a single-pointer alternative: "Make cover" / "Move left/right" in a per-photo menu. Sheet swipe-down already has alternatives (scrim tap, Esc, safe button), which is good.

**m9 · 3.3.1 / 4.1.3 · Loading, Reconnecting, Blocked (OQ-3).** When Bid or Buyout is disabled, the reason must be perceivable in text next to the bar and exposed in the button's semantics: "Bid unavailable, reconnecting". For a blocked buyer, any wording chosen for OQ-3 must still be a text message, not a silently inert button.

**m10 · 4.1.2 (mockup hygiene) · seller-screens.html, buyer-bidding.html.**
- All controls are `<div>`s with no role or tabindex.
- `role="switch"` (Buyout toggle) has no accessible name. Use `aria-labelledby` pointing at its label.
- ⋯ labels read "More for J. Cruz", but the spec says "More actions for J. Cruz". Align to the spec.
- The buyer mockup's footer note is stale ("stepper 44×48", "0–2px radius"). After Round 3 it's 48×48 with 6–10px radii.

Fix these so the mockups can't be copied as reference markup.

**m11 · Documentation accuracy · DESIGN.md › Colors; EXPERIENCE.md › Accessibility Floor.** The claimed ratios are wrong: ink-dark on surface-dark is 16.97, not 17.4; ink-secondary-dark is 6.91, not 7.8. "All text pairs ≥4.5:1 in both themes" is false until M3 is fixed. Replace the claims with the measured table above.

**m12 · 1.4.4 (advisory) · typography.label-caps 10px, status / pill 11px, tab labels 11px.** These are legal under AA but very small for an older, phone-first audience. Use a 11px minimum for label-caps and 12px for tab labels, and confirm none of them clip at 200%.

## 3. What already meets the bar

- **Targets:** every interactive element is at least 44px, and the stepper, Bid and Buyout are 48px (2.5.8 met with margin). Ladder rows are informational and their only target is a 44px ⋯.
- **Colour + word/symbol:** LIVE, ● OPEN, ▲, ✓, chip words, bold banner phrases, and a bold leader row (1.4.1).
- **Outbid and own state are assertive and announced once; others' bids are polite and coalesced.** The countdown isn't announced every second.
- **Reduced motion** removes the flash, row flash and count-up, and keeps the ▲ text.
- **Sheets** trap focus, return it to the trigger, and Esc goes to the safe option. Modal depth is one.
- **Validation messages** name the exact fix, with an ⚠ glyph, a 2px border and text (3.3.1, 3.3.3).
- **No confirmation on ordinary bids,** but Buyout and Close all have confirm sheets. That's appropriate for 3.3.4-style financial commits.

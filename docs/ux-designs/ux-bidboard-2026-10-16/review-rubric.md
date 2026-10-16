# Spine Pair Review — BidBoard

Rubric-walker lens (`references/validate.md`), run 2026-10-16. Canonical record: `.memlog.md`. Visual evidence: `mockups/buyer-bidding.html`, `mockups/seller-screens.html`, `mockups/color-themes.html`. Severity scale used here: **blocker / major / minor** (blocker = a downstream consumer can't build the affected slice correctly without a decision; major = a consumer will build something wrong or inconsistent unless fixed; minor = hygiene).

## Overall verdict

The pair is well above average: the memlog decisions are carried faithfully, every colour token has a hex and a light/dark pair, the four Key Flows have named protagonists and real climaxes, and DESIGN.md is in canonical order. What stops it being a clean build contract is the **bidding core**: four rules the server must enforce (first valid bid, custom-amount limits, buyout semantics, anti-snipe scope) are still open — and one of them (OQ-7) is answered *untagged* elsewhere in the same file — plus a missing bid-submission state model (pending / failed) and a **Facebook-platform feasibility risk** in the Messenger hand-off that no OQ captures. Component and state coverage are thin around the screens that weren't mocked (Sign-in sheet, My bids, Win screen, Account, drafts).

## 1. Flow coverage — adequate

Checked: EXPERIENCE.md has no `sources:` frontmatter (there is no upstream PRD; the memlog is the source), so flows were checked against the memlog's IA list and core problem. Four flows; all have a named protagonist, numbered steps and a climax. Flows 1 and 2 have failure paths; 3 and 4 don't.

### Findings
- **major** Flows 2 and 3 contradict each other on the same batch. Flow 3 has Liza close Batch 0416 at **8:55 PM** via Close all (Seiko included, Marco a winner); Flow 2 has Marco bidding on the Seiko in that batch from **8:57 PM** until the clock runs out after an anti-snipe extension. Both can't be true, and flows are the acceptance-test oracle downstream. (EXPERIENCE.md § Key Flows, Flow 2 heading + steps 4–7; Flow 3 heading + steps 3–5). *Fix:* move Flow 3 to a different batch or day, or make Flow 3 happen after Flow 2's close and use Close now on remaining open items instead.
- **major** Flow 2 step 6 says the Extended banner shows "and the Winning banner returns", but Component Patterns says **at most one banner, priority Outbid > Extended > Winning > Open**, and no banner lifetime is defined — so under the stated rule Winning can never return while Extended is up. (EXPERIENCE.md § Component Patterns › Status banner; § Key Flows Flow 2 step 6). *Fix:* define how long Extended persists (e.g. until the next bid or N seconds) and what replaces it; align the flow wording.
- **minor** Flows 3 and 4 have no failure path (e.g. Messenger hand-off fails / winner has no reachable profile; Offer to next bidder declined). (EXPERIENCE.md § Key Flows). *Fix:* add one failure line each once OQ-2 and the Messenger feasibility question (see §4) are answered.
- **minor** Flow 2 step 8 lists the Orient as "Outbid" while the Flow 2 buyout alternative has Marco buy the Orient out. It's labelled an alternative, but a reader extracting test data will trip on it. *Fix:* use a different item for the buyout alternative.

## 2. Token completeness — adequate

Checked: every `colors` key has a 6- (or 8-) digit hex and a `-dark` partner; every `{path.to.token}` in both files resolves (incl. `{colors.scrim-dark}`, `{motion.flash-duration}`, `{spacing.desktop-nav}`, `{components.input-error}`, `{components.price-tick}`). Contrast recomputed for 26 pairs.

### Findings
- **major** **Non-text contrast fails WCAG 2.2 AA 1.4.11.** `{colors.border}` #E4E4E7 on `{colors.surface}` #FFFFFF is **1.27:1**, and it is the only boundary on `input`, `button-quiet`, `bid-stepper-button`, `increment-chip`, `option-card` (unselected) and `toggle` track-off. Component boundaries need ≥ 3:1. The Accessibility Floor claims WCAG 2.2 AA across both themes. (DESIGN.md frontmatter `components.input/button-quiet/increment-chip/option-card/toggle/bid-stepper-button`; EXPERIENCE.md § Accessibility Floor). *Fix:* add a `border-strong` / `border-strong-dark` token (≥ 3:1 against surface and background) for interactive boundaries; keep `border` for decorative hairlines only.
- **major** **No disabled-state tokens or visual spec**, yet disabled is load-bearing: Bid/Buyout disabled while loading and reconnecting, `−` disabled at the floor, Bid disabled below the next valid bid, Publish/Save blocked by validation. (DESIGN.md frontmatter `colors`/`components`; referenced in EXPERIENCE.md § State Patterns Loading/Offline, § Interaction Primitives Custom amount, § Key Flows Flow 1 failure). *Fix:* add `ink-disabled`, `surface-disabled` (+ `-dark`) and a `-disabled` variant for `button-bid`, `button-buyout`, `button-primary`, `bid-stepper-button`.
- **major** **Font stacks are CSS-only and unbuildable as written in Flutter.** `typography.body.fontFamily` and `typography.figure.fontFamily` are browser stacks (`-apple-system`, `ui-monospace`, `SF Mono` …). Flutter web (CanvasKit) does not fall back to platform fonts, Android has no `ui-monospace`, and tabular numerals need `FontFeature.tabularFigures()` on a font that actually ships them. The "every figure is tabular" rule is a brand pillar ("figures never jitter"). (DESIGN.md frontmatter `typography.body`, `typography.figure`, § Typography). *Fix:* name the bundled families for Flutter (e.g. a sans and a mono with tabular figures, served as assets), keep the CSS stack as a `note`, and state that `letterSpacing` em values convert to logical px.
- **minor** Stated contrast ratios are slightly off: `{colors.ink-secondary-dark}` on `{colors.surface-dark}` is **6.91:1**, not 7.8:1 (7.76 is against `background-dark`); `{colors.ink-dark}` on `{colors.surface-dark}` is **16.97:1**, not 17.4:1. All still pass. (DESIGN.md § Colors › Neutrals). *Fix:* correct the numbers.
- **minor** `{colors.success}` on `{colors.success-tint}` is 4.14:1 — fine for the chip dot/border (non-text), but nothing forbids it as text. (DESIGN.md § Colors table). *Fix:* add "never as text on its tint; use `success-label`" to the Success row's "Never used on".
- **minor** Several component values are raw literals where a token exists or should: `bid-stepper-button.size` (48px = `spacing.target-primary`), `bid-stepper-button.fontSize: 20px`, `status-chip` 11px/700 (= `typography.status`), `trust-signal` 11px/600, `button-buyout.fontSize: 14px`, `confirmation-sheet.radius: '10px 10px 0 0'` (= `rounded.lg`), all three shadows, "15px on some seller bars" button size, the 150px photo block, `status-banner.border-left: '4px solid (status colour)'` (placeholder, not a value). (DESIGN.md frontmatter `components`; § Typography table; § Layout & Spacing). *Fix:* reference existing tokens; add `elevation` values as named tokens (or a `note`) and a `typography.button-md` for the 15px case.

## 3. Component coverage — thin

Checked: every component named in either file against a DESIGN.md frontmatter entry + Components prose row, and an EXPERIENCE.md Component Patterns row.

### Findings
- **major** Unmocked surfaces have **no component spec at all** in either file: Sign-in sheet (Facebook button, explanation, cancel), Win screen, My bids list/row, Results **winner card** ("four winner cards", Flow 3), dashboard auction row, Create step progress bar (`rounded.xs` is defined only for it), photo gallery / photo counter on the item page, header strip, desktop left rail, desktop item-table header, avatar (`rounded.full` "Avatars"), skeleton strip, "Reconnecting…" line, copy confirmation feedback. (DESIGN.md § Components; EXPERIENCE.md § Component Patterns; IA tables mark Sign-in sheet / My bids / Win screen "not mocked"). *Fix:* add at least a frontmatter entry + one-line visual spec and a behaviour row for each; or render one more mockup sheet covering Sign-in sheet, Win screen and My bids (the three buyer surfaces a first sprint will hit).
- **minor** Components specified visually but with no behaviour row: `tab-bar`, `toggle`, `option-card`, `photo-slot` (reorder? change cover? upload limits/format?), `increment-chip` (can a seller enter a custom increment? fixed ₱50/100/500 is category-specific for a category-agnostic product), `empty-state`, `focus-ring`, `button-quiet`, `button-destructive`. (EXPERIENCE.md § Component Patterns). *Fix:* add behaviour rows; decide custom increment.
- **minor** Component names drift between files: "Bid ladder row" vs "Ladder"; "Item row (control room)" vs "Control-room item row"; "Neutral trust signal" vs "Trust signal"; "Overflow menu" vs "⋯ menu"; "Close-all sheet" vs "Close all confirmation" (mockup). *Fix:* pick one name each and use it in both files.

## 4. State coverage — thin

Checked: each IA surface for loading, empty, error, offline, permission-denied, and domain states.

### Findings
- **blocker** **Messenger hand-off feasibility is unverified and not an OQ.** The post-win contract ("seller messages the winner on Messenger via their Facebook profile"; ⋯ → "View Facebook profile" / "Message on Messenger"; Results "Message on Messenger") assumes BidBoard can get a usable profile or Messenger link from Facebook Login. Facebook Login returns an app-scoped user ID; the profile `link` field sits behind an App-Review-gated permission and usernames/m.me handles aren't exposed. If that permission isn't granted, the whole post-win step — the product's resolution of the "who won what" problem — has no fallback. (EXPERIENCE.md § Foundation "After the win", § Component Patterns ⋯ menu, § Key Flows Flow 3 climax; memlog `[change]` "Winner contact = Messenger via FB profile"). *Fix:* add as an OQ and a pre-architecture spike; define the fallback state (e.g. winner shares a contact handle at win time, or in-app message relay) and the failure state on Results.
- **major** **Bid submission has no pending or failed state.** Only "overtaken while tapping" is covered. Missing: what the Bid button shows between tap and server ack (optimistic row? spinner? locked?), and what happens on timeout/5xx/rate-limit — "Reliable" is a brand pillar and this is the money-committing action. (EXPERIENCE.md § State Patterns; § Interaction Primitives "Bid — one tap, no confirm"). *Fix:* add Submitting and Bid-failed rows (button state, banner/inline copy in the plain voice, retry rule, double-tap guard).
- **major** **Drafts have no home.** Create has **Save draft**, but the dashboard segments are Live · Scheduled · Ended and no surface lists drafts, edits a scheduled auction, or cancels/deletes one. (EXPERIENCE.md § IA Seller surfaces Dashboard + Create · Batch items; § Component Patterns Segmented control; mockup seller-screens.html §2 Save draft). *Fix:* add a Drafts segment or row group, and decide edit/cancel rules for Scheduled and Live auctions (at minimum: can a live listing's buyout/increment change? — affects bid validation).
- **major** **Sign-in edge states missing:** user cancels the Facebook dialog; Facebook account has **no email** (phone-registered accounts — memlog says alerts go to the FB profile email, so outbid/won emails silently fail); Facebook Login inside the **Facebook in-app browser**, which is where every buyer arrives from the shared link, and must survive the redirect while preserving the pending bid. (EXPERIENCE.md § State Patterns "Sign-in required at first bid"; § Notifications). *Fix:* add states for cancelled sign-in and no-email (ask for an email, or explain alerts are unavailable); note the in-app-browser constraint in Foundation.
- **major** No **not-found / unavailable** state for a shared link: deleted or cancelled auction, ended batch opened days later, mistyped URL, seller account removed. (EXPERIENCE.md § State Patterns; IA Batch page "Reached from shared link"). *Fix:* add an Unavailable state for the Batch page and Item page.
- **major** No **seller identity/onboarding** surface: Flow 1 step 1 "signs in with Facebook and lands on the Dashboard", but the IA only has the buyer Sign-in sheet. How does a Facebook user become a seller, and where does the seller's shop name ("Liza's Timepieces") and URL slug (`bidboard.app/liza/…`) get set? (EXPERIENCE.md § IA Seller surfaces; Flow 1 step 1). *Fix:* add a seller sign-in / first-run surface or fold it into OQ-5/OQ-6 and say so.
- **minor** Missing smaller states: My bids empty; control room with no bids yet; photo upload progress/failure; publish failure; "Copied" feedback; seller bidding on their own item (shill-bid rule); buyer sign-out (buyers have no account surface). *Fix:* add rows or explicitly defer them.
- **minor** Internal contradiction: OQ-7 asks whether the first valid bid is base or base + increment, but **Interaction Primitives states it untagged** ("or base price when there are no bids") and State Patterns says "stepper starts at base" (tagged). (EXPERIENCE.md § Interaction Primitives Bid stepper; § State Patterns "Empty — no bids yet"; § Open Questions OQ-7). *Fix:* tag the Interaction Primitives line `[ASSUMPTION]` until OQ-7 closes. (Severity minor on the wording; the underlying OQ is a blocker — see Open Questions triage.)

## 5. Visual reference coverage — adequate

Checked: `mockups/` (3 files), `wireframes/` (absent), `imports/` (empty), `.working/` (6 files, not linked — expected).

### Findings
- **minor** DESIGN.md links mockups only once at the top; Components prose never points at the mockup section that shows each component (EXPERIENCE.md IA tables do this well with a Mockup column). (DESIGN.md § Components). *Fix:* add "see buyer-bidding.html · Hero" style pointers for action bar, banners, control-room row, close-all sheet, blocked list.
- **minor** Memlog names `.working/direction-ticker-rounded.html` as the chosen direction; the promoted file is `buyer-bidding.html` but neither spine says it *is* the Ticker-rounded direction. *Fix:* one clause in DESIGN.md § Brand & Style.
- Spines-win-on-conflict is stated once per file ✓. Rejected themes in `color-themes.html` are called out ✓. No orphans. Mockup chrome colours `#D4D4D8` / `#3F3F46` are page-presentation only and correctly absent from tokens.

## 6. Bloat & overspecification — adequate

### Findings
- **minor** EXPERIENCE.md § Trust & Bogus Buyers point 3 carries editorial argument ("fairness and defamation risk, and an invitation to abuse in small communities") — rationale belongs in the memlog; EXPERIENCE.md prose should state the rule. *Fix:* cut to the rule plus a one-line reason.
- **minor** DESIGN.md § Typography table restates every frontmatter typography token's size/weight; acceptable as a reading aid, but it already disagrees with frontmatter once (the untokenised "15px on some seller bars"). *Fix:* keep the table but make it source-only from tokens.

## 7. Inheritance discipline — adequate

### Findings
- **minor** No `sources:` frontmatter in either file. Memlog is the source and there's no PRD; say so explicitly (`sources: [.memlog.md]`) so a consumer knows there's no upstream to reconcile against.
- **minor** Surface-name drift from the memlog IA: memlog "Auction page" → spine "Batch page (auction page)"; for a **single-item** auction the link goes straight to Item live bidding, so "Auction page" no longer names one thing. *Fix:* pick "Batch page" everywhere and note single-item links skip it.
- **minor** DESIGN.md frontmatter carries non-spec keys: `status`, `updated`, a top-level `motion` group, `typography.figure.note`, and 8-digit alpha hex in `scrim` / `scrim-dark`. All resolve inside this file, but a spec-strict DESIGN.md linter/resolver may drop them — and `motion.flash-duration` is referenced from EXPERIENCE.md. (DESIGN.md frontmatter). *Fix:* keep them if your consumer tolerates extensions, but document `motion` as an extension in a frontmatter comment; express scrims as `{colors.ink}` + opacity `note` if strict tooling is planned.

### Invented decisions (not in memlog, not visible in mockups, not tagged `[ASSUMPTION]`)
- **major** "**Banned: auto-bidding or proxy bids (not in scope)**" — a scope decision with no memlog entry. (EXPERIENCE.md § Interaction Primitives Banned). *Fix:* log it as a decision or tag `[ASSUMPTION]`.
- **major** "**One tap places the bid — no confirmation sheet for ordinary bids**" — load-bearing for a money-committing action, consistent with the Ticker mockup's single Bid button but never decided. (EXPERIENCE.md § Component Patterns Bid button; § Interaction Primitives). *Fix:* log as a decision.
- **minor** Status banner priority order (Outbid > Extended > Winning > Open); control-room rows "sorted by most recent activity"; trust signal shown "only when ≥ 1 unpaid win"; "Won" email also sent on Close all and "closed early" email includes the bidder's outcome; Offer to next bidder "at that bidder's last bid"; sheet dismiss (scrim/swipe/Esc) = safe option; stepper auto-reset when overtaken (tagged in State Patterns but untagged in Component Patterns). *Fix:* tag `[ASSUMPTION]` or log each.
- **minor** Foundation "Out of scope: Facebook integration beyond sign-in and sharing a link" omits the Messenger/profile hand-off that § Responsive & Platform and the ⋯ menu rely on. *Fix:* "…beyond sign-in, link sharing and Messenger/profile hand-off".

### Memlog decisions missing or contradicted
None missing. Every `[decision]`/`[change]` entry is carried: Ledger hexes match exactly; radii 6/8/10 + square strips; action-bar layout; Facebook-only sign-in; email alerts; anti-snipe 30s no cap; custom amounts ≥ next valid bid; per-seller silent block; 90-day neutral signal; full vs shortened names; early Close all emails; plain voice. "Steal" correctly survives only in the Facebook caption house rule.

## 8. Shape fit — strong

DESIGN.md sections are in canonical order with none omitted. EXPERIENCE.md has all eight required defaults plus Responsive & Platform (triggered: phone + laptop). Inspiration & Anti-patterns is absent — defensible (memlog: "No reference apps"), though the Facebook comment thread is a clear anti-pattern that's only implied. Invented sections **Trust & Bogus Buyers** and **Notifications** earn their place.

## Open Questions triage (for building the Flutter app)

**Phase-blocking — decide before architecture / first build sprint** (each changes server-side bid validation, the auction state machine or the data model):
- **OQ-1** Buyout semantics — closes the item? still offered once bids pass it? Defines the item state machine.
- **OQ-3** Blocked buyer's bid outcome — "silent" forces an API choice (reject with a neutral error vs. accept-and-discard, which would corrupt other viewers' ladders).
- **OQ-7** First valid bid = base or base + increment — the core validation rule (and currently contradicted in-file).
- **OQ-16** Custom amounts: increment alignment and upper bound (at/over buyout) — the other half of bid validation.
- **OQ-18** Anti-snipe extends the item or the whole batch clock — per-item vs per-batch end time in the schema and the realtime fan-out.
- **New, not yet an OQ:** Messenger/profile hand-off feasibility under Facebook Login permissions (see §4 blocker).

**Blocking only for their own epic** (decide before that epic's stories are written; won't stall architecture):
- **OQ-2** Offer to next bidder (Results epic — offer entity, acceptance, expiry).
- **OQ-17** Name-shortening rule (must be enforced server-side before any buyer-facing ladder API ships).
- **OQ-4** My bids entry point/contents; **OQ-9** pre-start buyer view (buyer epic).
- **OQ-19** Close now / Close & award confirmation + email (seller control-room epic).

**Safe to defer** (pick a default, ship, tune): OQ-5 Account contents, OQ-6 dashboard empty state, OQ-8 reconnect timeout (architecture must support resync regardless), OQ-10 countdown announcement thresholds, OQ-11 laptop breakpoint, OQ-12 buyer laptop layout, OQ-13 theme selection (default: follow system), OQ-14 outbid email throttling (before launch, not before build), OQ-15 seller notifications, OQ-20 currency (store ISO currency code + minor units from day one and it stays deferrable).

## Mechanical notes

- All `{path.to.token}` references in both files resolve; no broken cross-refs.
- No Mermaid blocks.
- Frontmatter: DESIGN.md has `name`, `description`, all five token groups, light/dark pairs complete (21 + 21 colours). EXPERIENCE.md frontmatter has `design: DESIGN.md` but no `sources`.
- Shapes table lists "photo counter" and "Avatars" — neither exists as a component.
- `rounded.xs` (2px) is used only by an unspecified step-progress component.

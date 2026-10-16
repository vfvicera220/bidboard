---
name: BidBoard
description: Live auction board for sellers who outgrew Facebook comment bidding. Ledger theme — a monochrome trading-ledger tool where colour only ever means status.
status: final
updated: 2026-10-16
sources: [.memlog.md]
# Extensions beyond the DESIGN.md spec, kept deliberately: `status`, `updated`, `sources`, and the
# top-level `motion` group (referenced from EXPERIENCE.md). `scrim` / `scrim-dark` are 8-digit hex
# (RGB + alpha); strict tooling can read them as {colors.ink} at 55% / black at 65%.
colors:
  # Light (default token names)
  background: '#F5F5F4'
  surface: '#FFFFFF'
  ink: '#18181B'
  ink-secondary: '#52525B'
  border: '#E4E4E7'
  border-strong: '#8A8A93'
  primary: '#18181B'
  on-primary: '#FFFFFF'
  accent: '#4F46E5'
  accent-tint: '#EEF2FF'
  focus: '#6366F1'
  live: '#D02222'
  on-live: '#FFFFFF'
  success: '#15803D'
  success-label: '#137638'
  success-tint: '#DEEDE4'
  warning: '#A16207'
  warning-tint: '#FEF3C7'
  danger: '#B91C1C'
  danger-tint: '#FEE2E2'
  surface-disabled: '#F4F4F5'
  ink-disabled: '#71717A'
  photo-placeholder: '#E0E0E1'
  scrim: '#18181B8C'
  # Dark (-dark suffix)
  background-dark: '#09090B'
  surface-dark: '#18181B'
  ink-dark: '#FAFAFA'
  ink-secondary-dark: '#A1A1AA'
  border-dark: '#2E2E33'
  border-strong-dark: '#6A6A72'
  primary-dark: '#FAFAFA'
  on-primary-dark: '#09090B'
  accent-dark: '#818CF8'
  accent-tint-dark: '#1E1B3A'
  focus-dark: '#818CF8'
  live-dark: '#F87171'
  on-live-dark: '#09090B'
  success-dark: '#4ADE80'
  success-label-dark: '#4ADE80'
  success-tint-dark: '#1F3429'
  warning-dark: '#FACC15'
  warning-tint-dark: '#2A2410'
  danger-dark: '#F87171'
  danger-tint-dark: '#2D1414'
  surface-disabled-dark: '#27272A'
  ink-disabled-dark: '#8A8A93'
  photo-placeholder-dark: '#313135'
  scrim-dark: '#000000A6'
typography:
  # Two bundled families only: Inter for words, JetBrains Mono for every figure [ASSUMPTION].
  # Both ship as Flutter font assets; figures also set FontFeature.tabularFigures().
  # letterSpacing em values convert to logical px (em x fontSize) in Flutter.
  body:
    fontFamily: 'Inter'
    fontSize: 13px
    fontWeight: '400'
    lineHeight: '1.4'
  body-strong:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 13px
    fontWeight: '700'
    lineHeight: '1.4'
  item-title:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 14px
    fontWeight: '700'
    lineHeight: '1.4'
  input:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.4'
  question:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 15px
    fontWeight: '700'
    lineHeight: '1.4'
  empty-title:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 16px
    fontWeight: '700'
    lineHeight: '1.4'
  sheet-title:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 18px
    fontWeight: '700'
    lineHeight: '1.3'
  label-caps:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 11px
    fontWeight: '600'
    lineHeight: '1.4'
    letterSpacing: 0.06em
  field-label:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 12px
    fontWeight: '600'
    lineHeight: '1.4'
  meta:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 12px
    fontWeight: '400'
    lineHeight: '1.4'
  tab-label:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 12px
    fontWeight: '600'
    lineHeight: '1.3'
  status:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 11px
    fontWeight: '700'
    lineHeight: '1.4'
    letterSpacing: 0.04em
  trust:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 11px
    fontWeight: '600'
    lineHeight: '1.4'
  pill:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 11px
    fontWeight: '800'
    lineHeight: '1.2'
    letterSpacing: 0.08em
  button:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 13px
    fontWeight: '700'
  button-buyout:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 14px
    fontWeight: '700'
  button-md:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 15px
    fontWeight: '700'
  button-lg:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 16px
    fontWeight: '700'
  stepper-glyph:
    fontFamily: '{typography.body.fontFamily}'
    fontSize: 20px
    fontWeight: '700'
  figure:
    fontFamily: 'JetBrains Mono'
    fontSize: 13px
    fontWeight: '700'
  figure-amount:
    fontFamily: '{typography.figure.fontFamily}'
    fontSize: 18px
    fontWeight: '700'
  countdown:
    fontFamily: '{typography.figure.fontFamily}'
    fontSize: 15px
    fontWeight: '700'
  price-hero:
    fontFamily: '{typography.figure.fontFamily}'
    fontSize: 30px
    fontWeight: '700'
    letterSpacing: -0.02em
  price-hero-compact:
    fontFamily: '{typography.figure.fontFamily}'
    fontSize: 24px
    fontWeight: '700'
    letterSpacing: -0.02em
rounded:
  none: 0px
  xs: 2px
  sm: 6px
  md: 8px
  lg: 10px
  full: 9999px
spacing:
  hairline: 1px
  '1': 2px
  '2': 4px
  '3': 6px
  '4': 8px
  '5': 10px
  '6': 12px
  '7': 14px
  '8': 16px
  '9': 24px
  gutter-mobile: 12px
  gutter-desktop: 16px
  target-min: 44px
  target-primary: 48px
  row-dense: 44px
  row-item: 56px
  tab-bar: 52px
  photo-block: 150px
  focus-offset: 2px
  desktop-nav: 200px
  desktop-detail: 340px
motion:
  flash-duration: 120ms
  flash-easing: none
  flash-min-interval: 1000ms
  extended-banner-lifetime: 10s
  copied-feedback: 2s
components:
  header-strip:
    background: '{colors.surface}'
    min-height: '{spacing.target-primary}'
    padding: '0 12px'
    border-bottom: '1px solid {colors.border}'
    title: '{typography.item-title}'
    sub: '{typography.meta}'
  photo-gallery:
    background: '{colors.photo-placeholder}'
    height: '{spacing.photo-block}'
    radius: '{rounded.lg}'
    margin: '8px 12px'
    nav-button: '{spacing.target-min} square, {colors.surface} fill, {colors.border-strong} frame'
  photo-counter:
    background: '{colors.surface}'
    foreground: '{colors.ink}'
    radius: '{rounded.sm}'
    padding: '2px 6px'
    typography: '{typography.status}'
  bid-action-bar:
    background: '{colors.surface}'
    border-top: '1px solid {colors.ink}'
    padding: '8px 12px 14px'
    layout: 'grid 48px 1fr 1fr 48px, gap 6px; row 1 = stepper, row 2 = Bid (cols 1-2) + Buyout (cols 3-4)'
    position: 'fixed to viewport bottom; sticky at the end of content when text scale >= 1.5 or viewport height < 500px'
    reason-line: '{typography.meta}, {colors.ink-secondary}, above row 2 when a control is disabled'
  bid-stepper-button:
    background: '{colors.background}'
    foreground: '{colors.ink}'
    border: '1px solid {colors.border-strong}'
    radius: '{rounded.md}'
    size: '{spacing.target-primary}'
    typography: '{typography.stepper-glyph}'
  bid-stepper-button-disabled:
    background: '{colors.surface-disabled}'
    foreground: '{colors.ink-disabled}'
    border: '1px solid {colors.border}'
  bid-amount:
    background: '{colors.surface}'
    border: '1px solid {colors.ink}'
    radius: '{rounded.md}'
    height: '{spacing.target-primary}'
    typography: '{typography.figure-amount}'
  bid-amount-error:
    border: '2px solid {colors.danger}'
    message: '{typography.field-label}, {colors.danger}, under the action bar row 1'
  button-primary:
    background: '{colors.primary}'
    foreground: '{colors.on-primary}'
    radius: '{rounded.md}'
    height: '{spacing.target-min}'
    height-bar: '{spacing.target-primary}'
    typography: '{typography.button}'
    typography-bar: '{typography.button-md}'
  button-primary-disabled:
    background: '{colors.surface-disabled}'
    foreground: '{colors.ink-disabled}'
  button-bid:
    background: '{colors.primary}'
    foreground: '{colors.on-primary}'
    radius: '{rounded.md}'
    height: '{spacing.target-primary}'
    typography: '{typography.button-lg}'
  button-bid-submitting:
    background: '{colors.primary}'
    foreground: '{colors.on-primary}'
    label: 'Placing ₱4,600…'
    spinner: '16px ring, {colors.on-primary}, static under reduced motion'
  button-bid-disabled:
    background: '{colors.surface-disabled}'
    foreground: '{colors.ink-disabled}'
  button-secondary:
    background: 'transparent'
    foreground: '{colors.ink}'
    border: '1.5px solid {colors.ink}'
    radius: '{rounded.md}'
    height: '{spacing.target-min}'
    typography: '{typography.button}'
  button-buyout:
    background: 'transparent'
    foreground: '{colors.ink}'
    border: '1.5px solid {colors.ink}'
    radius: '{rounded.md}'
    height: '{spacing.target-primary}'
    typography: '{typography.button-buyout}'
  button-buyout-disabled:
    background: '{colors.surface-disabled}'
    foreground: '{colors.ink-disabled}'
    border: '1.5px solid {colors.border}'
  button-quiet:
    background: 'transparent'
    foreground: '{colors.ink}'
    border: '1px solid {colors.border-strong}'
    radius: '{rounded.md}'
    height: '{spacing.target-min}'
    typography: '{typography.button}'
  button-destructive:
    background: 'transparent'
    foreground: '{colors.danger}'
    height: '{spacing.target-min}'
    typography: '{typography.button}'
  price-block:
    background: '{colors.surface}'
    padding: '10px 12px'
    border-bottom: '1px solid {colors.border}'
    label: '{typography.label-caps}'
    value: '{typography.price-hero}'
  price-tick:
    foreground: '{colors.success}'
    typography: '{typography.figure}'
    glyph: '▲ +(increment)'
    flash: '{motion.flash-duration}, no easing'
  live-pill:
    background: '{colors.live}'
    foreground: '{colors.on-live}'
    radius: '{rounded.sm}'
    padding: '4px 8px'
    typography: '{typography.pill}'
    dot: '7px circle, {colors.on-live}'
  open-status:
    foreground: '{colors.accent}'
    typography: '{typography.status}'
    glyph: '● OPEN'
  countdown:
    foreground: '{colors.live}'
    typography: '{typography.countdown}'
    format: 'mm:ss'
  reconnecting-line:
    foreground: '{colors.ink-secondary}'
    typography: '{typography.meta}'
    glyph: '↻ Reconnecting…'
    position: 'replaces the countdown in the clock strip'
  ladder-row:
    background: '{colors.surface}'
    padding: '6px 12px'
    border-bottom: '1px solid {colors.border}'
    amount: '{typography.figure}'
    when: '{colors.ink-secondary}'
  ladder-row-leader:
    fontWeight: '700'
    edge: 'inset 3px left, {colors.success}'
  ladder-pause:
    base: '{components.button-quiet}'
    label: 'Pause ladder / N new bids · Resume'
  status-banner:
    foreground: '{colors.ink}'
    border-left-width: 4px
    radius: '{rounded.md}'
    padding: '10px 12px'
    margin: '8px 12px'
    typography: '{typography.body}'
  status-banner-failed:
    background: '{colors.danger-tint}'
    rule: '{colors.danger}'
  status-banner-outbid:
    background: '{colors.danger-tint}'
    rule: '{colors.danger}'
  status-banner-extended:
    background: '{colors.warning-tint}'
    rule: '{colors.warning}'
  status-banner-winning:
    background: '{colors.success-tint}'
    rule: '{colors.success}'
    label: '{colors.success-label}'
  status-banner-open:
    background: '{colors.accent-tint}'
    rule: '{colors.accent}'
  blocked-notice:
    background: '{colors.surface-disabled}'
    foreground: '{colors.ink}'
    radius: '{rounded.md}'
    min-height: '{spacing.target-primary}'
    typography: '{typography.body}'
    position: 'replaces the bid action bar rows'
  stats-grid:
    background: '{colors.border}'
    cell-background: '{colors.surface}'
    gap: '{spacing.hairline}'
    radius: '{rounded.lg}'
    margin: '8px 12px'
    cell-padding: '6px 8px'
    label: '{typography.label-caps}'
    value: '{typography.figure}'
    columns: '4 (3 on narrow summaries; 2 x 2 at text scale >= 1.5)'
  item-row:
    background: '{colors.surface}'
    min-height: '{spacing.row-item}'
    padding: '8px 12px'
    columns-mobile: 'title 1fr · bid auto · count auto · status auto (status stacks under bid at text scale > 1.3)'
    columns-desktop: 'title 1fr · leader auto · bid auto · count auto · status auto'
    border-bottom: '1px solid {colors.border}'
  item-row-flash:
    edge: 'inset 3px left, {colors.success}'
    tick: '{components.price-tick}'
  item-row-selected:
    background: '{colors.background}'
    edge: 'inset 3px left, {colors.ink}'
  table-header:
    background: '{colors.background}'
    foreground: '{colors.ink-secondary}'
    typography: '{typography.label-caps}'
    border-bottom: '1px solid {colors.border}'
    padding: '6px 12px'
  batch-lot-row:
    background: '{colors.surface}'
    min-height: '{spacing.row-dense}'
    columns: '1fr auto auto'
    padding: '0 10px'
  my-bids-row:
    background: '{colors.surface}'
    min-height: '{spacing.row-item}'
    padding: '8px 12px'
    border-bottom: '1px solid {colors.border}'
    title: '{typography.item-title}'
    sub: '{typography.meta}, {colors.ink-secondary} (shop · your bid)'
    amount: '{typography.figure}'
    status: '{typography.status}'
  dashboard-auction-row:
    background: '{colors.surface}'
    min-height: '{spacing.row-item}'
    padding: '8px 12px'
    border-bottom: '1px solid {colors.border}'
    title: '{typography.item-title}'
    sub: '{typography.meta}'
    right: '{components.live-pill} + {components.countdown}, or {typography.meta} "Starts Fri 8:00 PM" / "Ended" / "Draft"'
  results-winner-card:
    background: '{colors.surface}'
    border: '1px solid {colors.border}'
    radius: '{rounded.lg}'
    padding: '10px 12px'
    margin: '8px 12px'
    title: '{typography.item-title}'
    winner: '{typography.body-strong}'
    amount: '{typography.figure}'
    chip: '{components.status-chip}'
    actions: '{components.button-quiet} Mark paid · Mark unpaid; {components.button-secondary} Offer to next bidder'
  win-panel:
    background: '{colors.surface}'
    padding: '16px 12px'
    title: '{typography.sheet-title}'
    amount: '{typography.price-hero-compact}'
    body: '{typography.body}'
    action: '{components.button-primary}, full width, height {spacing.target-primary}'
  status-chip:
    foreground: '{colors.ink}'
    radius: '{rounded.sm}'
    min-height: 24px
    padding: '2px 8px'
    typography: '{typography.status}'
    dot: '8px circle, status colour'
  status-chip-awaiting:
    background: '{colors.warning-tint}'
    border: '{colors.warning}'
  status-chip-paid:
    background: '{colors.success-tint}'
    border: '{colors.success}'
  status-chip-unpaid:
    background: '{colors.danger-tint}'
    border: '{colors.danger}'
  status-chip-no-bids:
    background: '{colors.surface}'
    border: '{colors.border-strong}'
    dot: '{colors.ink-secondary}'
  status-chip-open:
    background: '{colors.accent-tint}'
    border: '{colors.accent}'
  trust-signal:
    foreground: '{colors.ink-secondary}'
    background: '{colors.background}'
    border: '1px solid {colors.border}'
    radius: '{rounded.sm}'
    padding: '2px 6px'
    typography: '{typography.trust}'
  confirmation-sheet:
    background: '{colors.surface}'
    scrim: '{colors.scrim}'
    radius: '{rounded.lg} {rounded.lg} 0 0'
    padding: '8px 12px 18px'
    shadow: '0 -4px 20px rgba(0,0,0,.25)'
    grabber: '40px x 4px, {colors.border-strong}'
    title: '{typography.sheet-title}'
  sign-in-sheet:
    base: '{components.confirmation-sheet}'
    action: '{components.button-primary} "Continue with Facebook", height {spacing.target-primary}'
    safe: '{components.button-secondary} "Not now"'
    note: '{typography.meta}, {colors.ink-secondary}'
  overflow-menu:
    background: '{colors.surface}'
    border: '1px solid {colors.border}'
    radius: '{rounded.lg}'
    width: 250px
    shadow: '0 8px 24px rgba(0,0,0,.18)'
    row-min-height: '{spacing.target-min}'
  segmented-control:
    track: '{colors.background}'
    frame: '1px solid {colors.border-strong}'
    radius: '{rounded.lg}'
    segment-radius: '{rounded.md}'
    segment-height: '{spacing.target-min}'
    label: '{typography.button}, {colors.ink-secondary}'
    selected-background: '{colors.surface}'
    selected-label: '{colors.ink}'
    selected-indicator: '2px solid {colors.ink} outline'
    selected-shadow: '0 1px 2px rgba(0,0,0,.12)'
  option-card:
    background: '{colors.surface}'
    border: '1px solid {colors.border-strong}'
    selected-border: '2px solid {colors.ink}'
    selected-indicator: 'filled radio dot, {colors.ink}'
    radius: '{rounded.lg}'
    min-height: 72px
  input:
    background: '{colors.surface}'
    border: '1px solid {colors.border-strong}'
    radius: '{rounded.md}'
    min-height: '{spacing.target-min}'
    typography: '{typography.input}'
  input-disabled:
    background: '{colors.surface-disabled}'
    foreground: '{colors.ink-disabled}'
    border: '1px solid {colors.border}'
  input-error:
    border: '2px solid {colors.danger}'
    message: '{typography.field-label}, {colors.danger}, prefixed ⚠'
  increment-chip:
    background: '{colors.surface}'
    border: '1px solid {colors.border-strong}'
    selected-background: '{colors.primary}'
    selected-foreground: '{colors.on-primary}'
    radius: '{rounded.sm}'
    min-height: '{spacing.target-min}'
    min-width: 60px
  toggle:
    track-on: '{colors.ink}'
    track-off: '{colors.border-strong}'
    knob: '{colors.surface}'
    knob-outline: '1px solid {colors.border-strong}'
    on-indicator: 'knob right with ✓ glyph in {colors.ink}'
    off-indicator: 'knob left, no glyph'
    size: '44px x 26px'
    radius: '{rounded.full}'
  step-progress:
    height: 4px
    radius: '{rounded.xs}'
    done: '{colors.ink}'
    todo: '{colors.border-strong}'
    label: '{typography.meta} "Step 1 of 2"'
  photo-slot:
    background: '{colors.photo-placeholder}'
    radius: '{rounded.lg}'
    min-size: '{spacing.target-primary}'
    cover-ring: 'inset 2px {colors.ink}'
    add: '1.5px dashed {colors.ink-secondary}'
    uploading: 'determinate bar, {colors.ink} on {colors.border-strong}'
    failed: '2px solid {colors.danger} frame + ⚠ "Tap to retry"'
  skeleton-strip:
    background: '{colors.photo-placeholder}'
    radius: '{rounded.sm}'
    motion: 'static — no shimmer'
  copy-feedback:
    label: '"Copied ✓" for {motion.copied-feedback}'
    foreground: '{colors.ink}'
  tab-bar:
    background: '{colors.surface}'
    border-top: '1px solid {colors.border}'
    height: '{spacing.tab-bar}'
    label: '{typography.tab-label}, {colors.ink-secondary}'
    active-label: '{colors.ink}, 700'
    active-indicator: '2px {colors.ink} bar above the active item'
  desktop-nav:
    background: '{colors.surface}'
    width: '{spacing.desktop-nav}'
    border-right: '1px solid {colors.border}'
    item-min-height: '{spacing.target-min}'
    label: '{typography.body}, {colors.ink-secondary}'
    active: '{colors.ink} 700 + inset 3px left {colors.ink} edge'
  empty-state:
    background: '{colors.surface}'
    border: '1px dashed {colors.border-strong}'
    radius: '{rounded.lg}'
    title: '{typography.empty-title}'
  focus-ring:
    outline: '3px solid {colors.focus}'
    offset: '{spacing.focus-offset}'
    note: 'offset gap is mandatory in both themes; dark uses {colors.focus-dark}'
  focus-ring-inset:
    outline: '3px solid {colors.focus}, drawn inside'
    gap: '{spacing.focus-offset} {colors.surface} inner gap'
    note: 'full-width strips, rows and clipped containers'
---

# BidBoard — Design

> Composition references: [mockups/buyer-bidding.html](mockups/buyer-bidding.html) (buyer hero, states, batch page), [mockups/seller-screens.html](mockups/seller-screens.html) (dashboard, create, share, control room, results, blocked), [mockups/color-themes.html](mockups/color-themes.html) — **Ledger is the only chosen theme; the other four themes in that file are rejected.** Where a mockup and this document disagree, this document wins. In particular, the mockups still show the old live red (#DC2626), the old focus colour, hairline control borders, and a **Message on Messenger** action on Results and in the ⋯ menu — all superseded by this document.

## Brand & Style

BidBoard exists because a Facebook comment thread is a terrible auction house. At a few hundred comments, nobody — not the seller, not the buyers — can tell who is winning, what the next bid is, or whether "mine 4500" landed before the clock ran out. BidBoard replaces that thread with a board that is **fast, easy to use, and reliable**: one glance tells you the price, the next bid, the ladder, and the clock.

The visual posture is a **trading ledger, softened**. The direction is *Ticker* — specifically the rounded Ticker variant chosen in the memlog (`.working/direction-ticker-rounded.html`), promoted as `mockups/buyer-bidding.html`: dense hairline grids, monospaced tabular figures, a price that ticks `▲ +100` on every bid, and one fixed action bar at the bottom. On top of that sits the *Ledger* palette — a monochrome tool in which colour is never decoration. Black, white and greys carry the whole interface; colour appears only when it is telling you something about state: live, winning, extended, outbid, open, paid, unpaid.

The corners were deliberately softened from the original 0–2px ticker: discrete controls and blocks are rounded (6/8/10px) so the board reads as a friendly consumer tool rather than a brokerage terminal, while full-width strips stay square so the ledger rhythm survives.

- **Fast** — everything a bidder needs fits above the action bar without scrolling. Hard cuts, no easing.
- **Easy to use** — one action bar, one stepper, and a button label that always states the exact amount you're about to commit.
- **Reliable** — every bid is a receipt row: who, how much, when. Figures never jitter because they are tabular.

The UI is category-agnostic: watches are the running example, but nothing in the visual language assumes a category.

## Colors

The Ledger palette is two neutral ramps plus a handful of status colours. Light and dark are both first-class; dark tokens carry the `-dark` suffix. All ratios below are measured (WCAG 2.x relative luminance).

**Neutrals — the whole interface**

- **Background (`{colors.background}` #F5F5F4 / #09090B)** — page ground behind surfaces, the stepper button fill, the segmented-control track, and the tinted ground of a selected control-room row.
- **Surface (`{colors.surface}` #FFFFFF / #18181B)** — every strip, row, card, sheet and the action bar.
- **Ink (`{colors.ink}` #18181B / #FAFAFA)** — primary text, the 1px rule above the action bar, the bid-amount frame, the selected-row edge, selected option borders. 17.72:1 on surface (light), 16.97:1 (dark).
- **Ink secondary (`{colors.ink-secondary}` #52525B / #A1A1AA)** — labels, timestamps, hints, the neutral trust signal. 7.73:1 on surface (light); 6.91:1 on surface and 7.76:1 on background (dark).
- **Border (`{colors.border}` #E4E4E7 / #2E2E33)** — **decorative only**: hairlines between rows, dividers, the stats-grid gap colour, card outlines whose content identifies them. Never the only boundary of something you can operate (1.27:1 / 1.31:1 — fails 1.4.11 for controls).
- **Border strong (`{colors.border-strong}` #8A8A93 / #6A6A72)** — **control boundaries**: inputs, option cards, unselected increment chips, the stepper frame, quiet buttons, the toggle-off track, the segmented-control frame, the No-bids chip, the sheet grabber. 3.42:1 on surface and 3.14:1 on background (light); 3.30:1 on surface and 3.71:1 on background (dark).
- **Primary (`{colors.primary}` #18181B / #FAFAFA) on `{colors.on-primary}`** — the Bid button and every primary action. Primary *is* ink: the brand's strongest colour is black (white in dark). 17.72:1 / 19.06:1.
- **Disabled (`{colors.surface-disabled}` #F4F4F5 / #27272A with `{colors.ink-disabled}` #71717A / #8A8A93)** — the fill and label of any disabled control. 4.40:1 / 4.35:1, so a disabled label stays readable even though 1.4.3 exempts it. Disabled is never shown by colour alone: a reason line in text sits next to it (see Components → Bid action bar).
- **Photo placeholder (`{colors.photo-placeholder}` #E0E0E1 / #313135)** — empty photo slots, loading photo blocks and skeleton strips.
- **Scrim (`{colors.scrim}` 55% ink / `{colors.scrim-dark}` 65% black)** — behind bottom sheets.

**Status — colour that means something**

| Token | Light / Dark | Means | Used on | Never used on |
|---|---|---|---|---|
| `{colors.live}` | #D02222 / #F87171 | Running *now*, time pressure | LIVE pill (with `{colors.on-live}`, 5.34:1 / 7.19:1), countdown, "Live" status text (5.34:1 on surface, 4.89:1 on background — passes on the selected control-room row) | Errors, decoration |
| `{colors.success}` | #15803D / #4ADE80 | You lead / bid landed / paid | `▲` price tick, leader row edge, flashed row edge, Paid chip border and dot, Winning banner rule, "You lead" | Primary buttons; never as text on `{colors.success-tint}` (4.14:1) — use `{colors.success-label}` |
| `{colors.success-label}` | #137638 / #4ADE80 | Success *text* on tint | Bold label in the Winning banner (4.72:1 / 7.62:1 on `{colors.success-tint}`) | — |
| `{colors.warning}` | #A16207 / #FACC15 | Changed / waiting | Extended banner rule, Awaiting payment chip | — |
| `{colors.danger}` | #B91C1C / #F87171 | Lost position / problem / irreversible | Outbid and Bid-failed banner rules, Unpaid chip, field errors, destructive menu labels | Fills behind buttons |
| `{colors.accent}` | #4F46E5 / #818CF8 | Open-ended (no clock) | `● OPEN` status, Open banner rule, Open chip | Primary actions, links |
| `{colors.focus}` | #6366F1 / #818CF8 | Keyboard focus | `{components.focus-ring}` only | Anything else |

Light focus `{colors.focus}` #6366F1 passes 3:1 in every light adjacency with or without the gap (4.47:1 surface, 4.09:1 background, 3.97:1 against ink/primary, 3.66–4.01:1 against light tints). Dark `{colors.focus-dark}` #818CF8 is 5.94:1 / 6.67:1 on surface / background but only 2.86:1 against `{colors.primary-dark}`, so the **2px offset gap is mandatory** in dark.

Tints (`{colors.success-tint}`, `{colors.warning-tint}`, `{colors.danger-tint}`, `{colors.accent-tint}`) are only ever grounds for **ink text** with a coloured rule or dot. Coloured text never sits on a tint below 4.5:1. Tints are fixed values, not derived from `{colors.live}`, so the live change touches no tint.

Avoid: brand colour on chrome, gradients, coloured primary buttons, red fills, and any status colour shown without a word or symbol beside it.

## Typography

Two bundled families, nothing else [ASSUMPTION — font choice is not in the memlog]:

- **Words — Inter** (`{typography.body.fontFamily}`), shipped as a Flutter font asset. Flutter web (CanvasKit) does not fall back to platform fonts, so the family must be bundled, not named as a system stack. The browser-only fallback for non-Flutter surfaces (emails) is `-apple-system, "Segoe UI", Roboto, Arial, sans-serif`.
- **Figures — JetBrains Mono** (`{typography.figure.fontFamily}`), bundled, with `FontFeature.tabularFigures()` set on every figure style. **Every** amount, count, countdown and time-left uses it, so digits never shift width when a price ticks or a clock counts down. Roboto Mono is an acceptable substitute; email fallback is `ui-monospace, Menlo, Consolas, monospace`.
- `letterSpacing` values are in em; convert to logical px (em × fontSize) in Flutter.

Sizes below are the frontmatter tokens; this table only says where they go.

| Role | Token | Where |
|---|---|---|
| Price hero | `{typography.price-hero}` / `{typography.price-hero-compact}` | Current bid on item page; compact in narrow/state views and the win panel |
| Bid amount | `{typography.figure-amount}` | Stepper amount |
| Countdown | `{typography.countdown}` | Clock beside LIVE pill, in `{colors.live}` |
| Figure | `{typography.figure}` | Ladder amounts, stats values, row bids, `▲` tick |
| Sheet title | `{typography.sheet-title}` | "Close all now?", "Sign in to bid", "You won." |
| Empty title | `{typography.empty-title}` | "No one is blocked" |
| Question | `{typography.question}` | "What are you selling?" |
| Item title | `{typography.item-title}` | Listing titles, row titles, header strip |
| Input | `{typography.input}` | Form field text |
| Body | `{typography.body}` / `{typography.body-strong}` | Base size and banners — the ticker is dense on purpose |
| Field label / meta | `{typography.field-label}` / `{typography.meta}` | Form labels, errors, hints, sub-lines, reason lines |
| Tab label | `{typography.tab-label}` | Seller bottom tab bar |
| Status / pill / trust | `{typography.status}` / `{typography.pill}` / `{typography.trust}` | Row statuses and chips; LIVE pill; trust signal |
| Label caps | `{typography.label-caps}` | Stats-grid labels, table headers, section headers |
| Button | `{typography.button}` / `{typography.button-buyout}` / `{typography.button-md}` / `{typography.button-lg}` | Default buttons; Buyout; full-width seller bars; Bid |
| Stepper glyph | `{typography.stepper-glyph}` | `−` and `+` |

Rules: no display serif, no italics, no all-caps sentences (caps only for ≤3-word labels). Currency symbol stays attached to the figure (`₱4,600`). No text style clamps the text scale.

## Layout & Spacing

Scale: 1 · 2 · 4 · 6 · 8 · 10 · 12 · 14 · 16 · 24 px (`{spacing.hairline}` … `{spacing.9}`). The ticker lives at the small end: rows pad 6–10px vertically, strips pad 8–12px.

- **Mobile gutter:** `{spacing.gutter-mobile}` (12px). Discrete blocks (photo, banner, stats grid) inset 12px from the edge; full-width strips (header, title, price, ladder, action bar) run edge to edge.
- **Vertical order on the item page:** header strip → photo gallery (`{spacing.photo-block}`) → title strip → LIVE/clock strip → status banner (when any) → price block → stats grid → ladder → flexible space → fixed action bar. This is also the focus order.
- **Row heights:** buyer lot rows and dense list rows `{spacing.row-dense}` (44px); seller item, control-room, dashboard, My bids rows `{spacing.row-item}` (56px); ladder rows ~36px (information only — their only tap target is the ⋯ button, which is 44px).
- **Targets:** every tappable thing ≥ `{spacing.target-min}` (44px); primary bars and the stepper `{spacing.target-primary}` (48px).
- **Fixed-bar clearance:** every scroll view under the action bar or tab bar carries bottom padding equal to the bar's measured height, so a focused element is always scrolled fully above it.
- **Text scale:** item-row columns are intrinsic, not fixed px; above 1.3× the status stacks under the bid. At ≥1.5× the stats grid becomes 2 × 2, and at ≥1.5× or a viewport under 500px high the action bar stops being fixed and sits sticky at the end of content, reached directly by the **Skip to bid** control, so it can never cover more than half the screen.
- **Laptop control room (~1100px):** three-column master–detail — nav `{spacing.desktop-nav}` (200px) · item table (flex) · selected item ladder `{spacing.desktop-detail}` (340px); gutter `{spacing.gutter-desktop}` (16px).
- Seller mobile uses a 4-item bottom `tab-bar`; buyers have no tab bar.

## Elevation & Depth

Hierarchy comes from **tone and hairlines**, not shadow: `{colors.background}` sits behind `{colors.surface}` strips separated by 1px `{colors.border}` lines. The action bar is lifted by a 1px **ink** rule, not a shadow.

Shadows exist only for things that genuinely float above the board:

| Layer | Shadow |
|---|---|
| Bottom sheet | `{components.confirmation-sheet.shadow}` over `{colors.scrim}` |
| Overflow (⋯) menu | `{components.overflow-menu.shadow}` |
| Selected segment | `{components.segmented-control.selected-shadow}` (decorative; the 2px ink outline carries the state) |

Nothing else casts a shadow. One floating layer at a time.

## Shapes

| Radius | Token | Applies to |
|---|---|---|
| 0 | `{rounded.none}` | Full-width strips: header, title, price block, ladder, item rows, action-bar container, tab bar, desktop nav |
| 2px | `{rounded.xs}` | Create-flow step progress bar (`step-progress`) |
| 6px | `{rounded.sm}` | Pills and status: LIVE pill, status chips, trust signal, increment chips, photo counter, skeleton bars |
| 8px | `{rounded.md}` | Controls: buttons, stepper, bid amount, inputs, banners, segments, ⋯ button, blocked notice |
| 10px | `{rounded.lg}` | Blocks: photo gallery, stats grid, option cards, photo slots, menus, empty state, results winner cards, sheet top corners |
| full | `{rounded.full}` | Status dots, toggle track and knob |

The logic: things you *tap* are 8px, things that *label* are 6px, things that *contain* are 10px, and things that *span* are square.

## Components

**Header strip** (`{components.header-strip}`) — shop name in `item-title`, batch or lot sub-line in `meta`, 44px back control where there is a back. Buyers see the shop name; sellers see the screen name.

**Photo gallery** (`{components.photo-gallery}`, `{components.photo-counter}`) — `{spacing.photo-block}` block, `{rounded.lg}`, swipe between up to 6 photos plus 44px previous/next buttons (single-pointer alternative). The counter "1/6" sits bottom-right on `{colors.surface}`. See buyer-bidding.html · Hero.

**Bid action bar** (`{components.bid-action-bar}`) — fixed to the bottom of the item page on `{colors.surface}` with a 1px `{colors.ink}` top rule. Row 1: `−` stepper · amount · `+` stepper (grid 48 / 1fr 1fr / 48). Row 2: **Bid ₱4,600** (`button-bid`, primary, left half) and **Buyout ₱8,000** (`button-buyout`, secondary outline, right half), equal width, both 48px. Buyout is absent when the listing has no buyout price, and hidden once any bid reaches the buyout price; Bid then takes the full row. When any control is disabled, a reason line in `meta` sits above row 2 ("Bid unavailable — reconnecting"). See buyer-bidding.html · Hero.

- **Submitting** (`{components.button-bid-submitting}`) — the Bid label becomes "Placing ₱4,600…" with a small static-or-spinning ring; stepper, amount and Buyout switch to their disabled variants until the server answers.
- **Disabled** (`{components.button-bid-disabled}`, `{components.button-buyout-disabled}`, `{components.bid-stepper-button-disabled}`, `{components.button-primary-disabled}`, `{components.input-disabled}`) — `{colors.surface-disabled}` fill, `{colors.ink-disabled}` label, hairline border; always paired with a reason in text.
- **Blocked** (`{components.blocked-notice}`) — both rows are replaced by one `{colors.surface-disabled}` block reading "You can't bid on this seller's auctions."

**Bid stepper** (`{components.bid-stepper-button}`, `{components.bid-amount}`) — 48×48 buttons on `{colors.background}` with a `{colors.border-strong}` frame, `stepper-glyph` type. The amount between them is framed in `{colors.ink}` and set in `{typography.figure-amount}`. The Bid button label always repeats the amount. A custom amount that breaks a rule switches the frame to `{components.bid-amount-error}` with the message directly under row 1.

**Price block with ▲ flash** (`{components.price-block}`, `{components.price-tick}`) — "CURRENT BID" in `label-caps` over the price in `price-hero`; the `▲ +100` tick sits right-aligned in `{colors.success}`. On each new bid the tick and price swap instantly with a `{motion.flash-duration}` colour flash, no easing, at most once per `{motion.flash-min-interval}` per element. With no bids the label reads "STARTING BID" over the base price. On the seller item view the label names the leader: "CURRENT BID · MARCO R.".

**LIVE pill** (`{components.live-pill}`) — `{colors.live}` fill, `{colors.on-live}` text, 7px dot, "LIVE" in `pill` type, `{rounded.sm}`. Always paired with the countdown.

**Open status** (`{components.open-status}`) — replaces pill + clock on open-ended listings: "● OPEN" in `{colors.accent}` with "no timer" in `ink-secondary`.

**Countdown** (`{components.countdown}`) — `mm:ss` in `countdown` type, `{colors.live}`. Seller rows append "left" ("02:14 left"). Each item in a batch has its own countdown because items close independently.

**Reconnecting line** (`{components.reconnecting-line}`) — "↻ Reconnecting…" in `meta`/`ink-secondary`, in place of the countdown while the live stream is down.

**Ladder row** (`{components.ladder-row}`) — Bidder · Amount (right, mono) · When (right, `ink-secondary`, "just now / 1m / 3m"). The leader row (`ladder-row-leader`) is bold with a 3px `{colors.success}` inset edge. Seller ladders add a 44px ⋯ button at the end of each row and show the trust signal beside a name. A **Pause ladder** control (`{components.ladder-pause}`) sits in the ladder header. See buyer-bidding.html · Hero; seller-screens.html §4 item ladder.

**Status banner** (`{components.status-banner}`) — ink text on a tint, 4px left rule in the variant's `rule` colour, `{rounded.md}`, inset 12px. One at a time:
- Bid failed — `status-banner-failed`: "**Your bid wasn't placed** — someone bid ₱4,700 first. Next bid: ₱4,800."
- Outbid — `status-banner-outbid`: "**You've been outbid** — bid ₱4,600 to retake the lead".
- Extended — `status-banner-extended`: "**Bid in the last 30s** — closing extended to 00:30".
- Winning — `status-banner-winning`: "✓ You're winning — you're the top bidder at ₱4,500" (label in `{colors.success-label}`).
- Open-ended — `status-banner-open`: "**Open** — Liza closes this manually".

See buyer-bidding.html · States A–C.

**Stats grid** (`{components.stats-grid}`) — 4 cells separated by 1px `{colors.border}` gaps inside a `{rounded.lg}` block. Buyer: Bids · Incr · Next · Buyout. Seller item: Bids · Incr · Base · Buyout. Control room: Bids · Bidders · With bids · Total. Dashboard and results use the same grid with 3–4 cells. A missing value shows "—".

**Control-room item row** (`{components.item-row}`) — title + leader name (or "—") · current bid (mono, bold) with `▲` tick under it when fresh · bid count · status word (or "No bids yet"). Fresh rows (`item-row-flash`) get a 3px `{colors.success}` edge for the flash; the selected row on laptop (`item-row-selected`) gets a 3px `{colors.ink}` edge on `{colors.background}`. Laptop tables carry a `{components.table-header}`. See seller-screens.html §4.

**Batch lot row** (`{components.batch-lot-row}`) — buyer batch page: Lot · Bid (mono) · Status word ("Live" `{colors.live}`, "Outbid" `{colors.danger}`, "You lead" `{colors.success}`, "Open" `{colors.accent}`, "Sold" / "Won" `ink-secondary` / `{colors.success}`). An extended lot shows its own countdown in the status column. See buyer-bidding.html · Secondary.

**My bids row** (`{components.my-bids-row}`) — item title; shop and "Your bid ₱4,900" in `meta`; current bid (mono); personal status word (same words and colours as the batch lot row).

**Dashboard auction row** (`{components.dashboard-auction-row}`) — batch or item title, item/bid counts in `meta`, and on the right either LIVE pill + countdown, "● OPEN", "Starts Fri 8:00 PM", "Ended" or "Draft". See seller-screens.html §1.

**Results winner card** (`{components.results-winner-card}`) — item title, winner's full name, amount (mono), status chip, then **Mark paid** · **Mark unpaid** (quiet) and, once Unpaid, **Offer to next bidder** (secondary). There is no Messenger action on this card: winners message the seller. See seller-screens.html §5 (its Message on Messenger button is superseded).

**Win panel** (`{components.win-panel}`) — "You won." in `sheet-title`, item title, winning amount in `price-hero-compact`, the line "Message Liza to arrange payment." and one full-width primary button **Message Liza on Messenger** that opens the seller's m.me link.

**Status chip** (`{components.status-chip}`) — ink label on tint, 1px status border, 8px status dot, `{rounded.sm}`: **Awaiting payment** (`status-chip-awaiting`), **Paid** (`status-chip-paid`), **Unpaid** (`status-chip-unpaid`), **No bids** (`status-chip-no-bids`), and **Open** (`status-chip-open`). The word is always present.

**Trust signal** (`{components.trust-signal}`) — "2 unpaid wins · 90d" in `trust` type, `{colors.ink-secondary}` on `{colors.background}` with a hairline border, `{rounded.sm}`. Deliberately grey: it is a fact, not a warning. Never red, never an icon, never shown to buyers.

**Confirmation sheet** (`{components.confirmation-sheet}`) — bottom sheet over `{colors.scrim}`, `{rounded.lg}` top corners, grabber, `sheet-title` question, one-sentence outcome in `ink-secondary`, a stats grid with the outcome in numbers, a "can't be undone" line where true, then the committing primary action (named with its count — "Close all 6 items") and the safe secondary ("Keep bidding open") directly below it. Used for Close all and Buyout. See seller-screens.html §4 Close all confirmation.

**Sign-in sheet** (`{components.sign-in-sheet}`) — confirmation-sheet layout: "Sign in to bid", one `meta` line on what is shared ("Liza sees your full name; other bidders see a short version."), primary **Continue with Facebook**, safe **Not now**.

**Overflow menu** (`{components.overflow-menu}`) — 250px, `{rounded.lg}`, 44px rows with a bold label and optional `meta` explanation. Destructive entries use `button-destructive` styling (danger label).

**Buttons**
- **Primary** (`{components.button-primary}`) — `{colors.primary}` fill, `{colors.on-primary}` label, `{rounded.md}`, 44px (48px and `button-md` type in full-width seller bars). One per bar.
- **Secondary** (`{components.button-secondary}`) — transparent with a 1.5px `{colors.ink}` outline. Buyout, Keep bidding open, Open Facebook, Copy, Unblock, Offer to next bidder, Not now.
- **Quiet** (`{components.button-quiet}`) — transparent with a 1px `{colors.border-strong}` outline for low-stakes row actions: Share link, Mark paid, Mark unpaid, Save draft, Pause ladder.
- **Destructive** (`{components.button-destructive}`) — `{colors.danger}` label, no fill; used for menu actions like **Block from my auctions**. Irreversible *bulk* commits (Close all) stay primary inside a confirmation sheet that says "Closing can't be undone."
- **Copy feedback** (`{components.copy-feedback}`) — after Copy, the button label reads "Copied ✓" for `{motion.copied-feedback}`.

**Form controls** — `input` (44px, `{colors.border-strong}` frame, `{rounded.md}`), `input-error` (2px `{colors.danger}` frame + `field-label` danger message prefixed ⚠, e.g. "⚠ Buyout must be above the base price (₱4,000)."), `input-disabled`, `increment-chip` (₱50 / ₱100 / ₱500; selected = primary fill), `toggle` (on = ink track, knob right with ✓; off = `{colors.border-strong}` track, outlined knob left, no glyph), `option-card` (radio cards; selected = 2px ink border + filled radio dot), `segmented-control` (Live · Scheduled · Ended · Drafts; selected = surface segment with a 2px ink outline, ink label), `step-progress` (two 4px bars, done ink / to-do `{colors.border-strong}`, plus "Step 1 of 2"), `photo-slot` (6 slots, first is the cover with a 2px ink ring; empty slot dashed `+`; uploading bar; failed ⚠ frame). The Seller profile form uses the same `input` controls. See seller-screens.html §2.

**Tab bar** (`{components.tab-bar}`) — four items, `tab-label` type; the active item is ink, bold, and marked by a 2px ink bar above it, so state never rests on colour.

**Desktop nav** (`{components.desktop-nav}`) — left rail, 44px items; active item is ink, bold, with a 3px ink inset edge.

**Skeleton strip** (`{components.skeleton-strip}`) — static `{colors.photo-placeholder}` bars in the final layout; no shimmer.

**Empty state** (`{components.empty-state}`) — dashed `{colors.border-strong}` block, symbol, `empty-title`, one explanatory sentence that says where the action actually lives. Also used for the Unavailable link page. See seller-screens.html §6.

**Focus ring** (`{components.focus-ring}`, `{components.focus-ring-inset}`) — 3px `{colors.focus}` outline with a mandatory `{spacing.focus-offset}` gap on every interactive element. Full-width strips, rows and anything inside a clipping container (photo gallery, banners, stats grid, ladder) use the inset variant so the ring is never clipped or pushed off-screen. The ring never touches an ink or primary fill directly.

## Do's and Don'ts

| Do | Don't |
|---|---|
| Keep the interface monochrome; colour only for status | Colour chrome, headers, or primary buttons |
| Pair every status colour with a word or symbol (LIVE, ▲, ✓, "Unpaid") | Rely on a red/green dot alone |
| Set every figure in JetBrains Mono with tabular figures | Mix proportional digits into prices, counts or clocks |
| Put the exact amount in the Bid button label | Use a bare "Bid" or "Place bid" |
| Flash new bids with a 120ms hard colour cut, at most once a second per element | Ease, bounce, slide or count up prices by default |
| Square full-width strips; 6/8/10px on discrete pills/controls/blocks | Round strips, or use pill-shaped buttons |
| Keep the trust signal grey and factual | Badge bidders red, add warning icons, or label them "bogus" |
| Show ink text on tints with a coloured rule | Put coloured text on a tint below 4.5:1 |
| Frame every control with `{colors.border-strong}` | Use the `{colors.border}` hairline as a control's only boundary |
| Show disabled with the disabled tokens *and* a reason in text | Grey out a control with no explanation |
| Keep the focus ring's 2px gap (inset on strips and clipped blocks) | Let the ring touch an ink fill or get clipped |
| Use shadow only for sheets and menus | Shadow cards or rows to create hierarchy |
| Design light and dark together from the token pairs | Ship a screen in one mode only |

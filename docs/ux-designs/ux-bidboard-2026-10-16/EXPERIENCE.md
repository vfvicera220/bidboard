---
name: BidBoard
status: final
updated: 2026-10-16
design: DESIGN.md
sources: [.memlog.md]
---

# BidBoard — Experience

> Composition references: [mockups/buyer-bidding.html](mockups/buyer-bidding.html), [mockups/seller-screens.html](mockups/seller-screens.html), [mockups/color-themes.html](mockups/color-themes.html) (Ledger only). There is no upstream PRD; `.memlog.md` is the only source. Where a mockup and this document disagree, this document wins.

## Foundation

BidBoard moves Facebook-group auctions off the comment thread. Facebook stays the **shop window** — sellers post a link in their groups — but every bid happens on BidBoard, where the price, the next valid bid, the ladder and the clock are always unambiguous.

- **One Flutter codebase** ships as mobile web (Flutter web) and an optional native app (Flutter mobile). Flutter web must enable its semantics tree at startup (see Accessibility Floor).
- **Buyers never install anything.** They tap a link in a Facebook post and bid in the browser they're already in — usually the **Facebook in-app browser**, so sign-in must survive that browser's redirect and keep the pending bid. The native app is optional and never required to bid.
- **Sellers** run auctions mostly from a phone, sometimes from a laptop browser.
- **Identity:** Sign in with Facebook only. No anonymous bidding. Google sign-in is deferred.
- **After the win:** the seller sets her Messenger link (`m.me/…`) once in her **Seller profile**. The winner's Win screen and won email say "Message Liza to arrange payment." with a button that opens that link. The seller does not contact the winner through BidBoard and BidBoard does not use the winner's Facebook profile for contact. No in-app payment.
- **Facebook profile links:** Facebook Login most likely cannot give BidBoard a usable profile or Messenger link for a buyer (that field sits behind an App-Review-gated permission). This is **unverified — resolve during build**. Nothing in this design depends on it.
- **Category-agnostic:** clothes, watches, collectibles — nothing in the flow assumes a category.
- `DESIGN.md` is the visual reference (Ledger theme, Ticker direction). This document is the behaviour.

**Out of scope:** payments; Facebook integration beyond sign-in, link sharing and opening the seller's Messenger link; Google sign-in (deferred); web push (later, optional); auto-bidding or proxy bids [ASSUMPTION].

## Information Architecture

### Buyer surfaces (phone browser first)

| Surface | Reached from | Purpose | Mockup |
|---|---|---|---|
| Batch page | Shared link in a Facebook post (e.g. `bidboard.app/liza/b/0416`). Single-item links skip it and open Item live bidding directly. | All lots in a batch with current bid, each lot's own status and the buyer's own standing per lot | buyer-bidding.html · Secondary |
| Item live bidding | Lot row on the batch page; direct link for single-item auctions; email links | Watch and bid on one item: price, stats, ladder, action bar | buyer-bidding.html · Hero + States A–H |
| Sign-in sheet | First tap on Bid or Buyout while signed out | Sign in with Facebook, then complete the bid that was interrupted | — (not mocked; spec in DESIGN.md) |
| Buyout confirmation sheet | Tap Buyout | Confirm buying the item outright at the buyout price | — (confirmation-sheet pattern) |
| My bids | Outbid / won emails; navigation entry (see OQ-2) | Every item the buyer has bid on and their status on each | — (not mocked; spec in DESIGN.md) |
| Win screen | Won email; the item page at close when the buyer leads; a confirmed buyout | "You won. Message Liza to arrange payment." with **Message Liza on Messenger** | buyer-bidding.html · State I |

### Seller surfaces

Mobile navigation is a bottom tab bar: **Auctions · Results · Blocked · Account**. Laptop navigation is a left rail: Dashboard · Control room · Results · Blocked bidders · Account.

| Surface | Reached from | Purpose | Mockup |
|---|---|---|---|
| Dashboard | Sign-in; Auctions tab | Today's numbers, **+ New auction**, Live · Scheduled · Ended · Drafts [ASSUMPTION — Drafts segment] | seller-screens.html §1 |
| Create · Step 1 Format | + New auction | Single item / Batch; Timed (date + time) / Open-ended | §2 Step 1 |
| Create · Step 2 Item form | Step 1 "Next: add items"; batch "+ Add item" / row tap | Photos (≤6, first is cover), title, description, base, increment, optional buyout | §2 Step 2 |
| Create · Batch items | Step 2 in batch mode | List of items; Save draft; Publish · N items (edit/cancel rules OQ-16) | §2 Batch variant |
| Seller profile | First **Publish** (required once); Account tab | Shop name, shop link (`bidboard.app/liza`), and **Messenger link** (`m.me/…`) shown to every winner | §7 (first publish + Account tab) |
| Share | Publish | Copy link, editable Facebook caption, Open Facebook, Go to control room | §3 |
| Live control room | Tapping a live dashboard row; Share → Go to control room | One row per item; batch stats; Share link; Close all | §4 batch overview (light/dark/laptop) |
| Item ladder | Tapping a control-room row (laptop: right-hand detail panel) | Full bid history with real names, trust signal, ⋯ menu, Close now | §4 item ladder |
| Open-ended item control | Tapping an open-ended dashboard row | Ladder without a clock; Close & award | §4 open-ended |
| Close-all sheet | Close all | Outcome in numbers; confirm or keep bidding open | §4 Close all confirmation |
| Results | Results tab; a batch ending | Per-item winner (full name), amount, payment status; **Mark paid** / **Mark unpaid** / **Offer to next bidder** | §5 |
| Blocked bidders | Blocked tab | Per-seller private blocklist; Unblock | §6 (list + empty) |

The **Account** tab holds the Seller profile; anything else it contains is open (OQ-3).

Modal depth is one: a sheet or menu sits over a surface, never over another sheet.

## Voice and Tone

Plain and direct. Say what happened, then what to do, with the real number. No exclamation marks, no hype, no blame.

| Context | Do | Don't |
|---|---|---|
| Outbid (banner) | "You've been outbid — bid ₱4,600 to retake the lead" | "Oh no! Someone beat you!" |
| Outbid (email / voice reference) | "You've been outbid. Next bid: ₱4,600." | "Bid war alert 🔥" |
| Winning | "✓ You're winning — you're the top bidder at ₱4,500" | "You're crushing it!" |
| Won | "You won. Message Liza to arrange payment." | "Congratulations!!" |
| Anti-snipe extension | "Bid in the last 30s — closing extended to 00:30" | "Anti-sniping protocol engaged" |
| Open-ended | "Open — Liza closes this manually" | "No end date" |
| Bid button | "Bid ₱4,600" | "Place bid" / "Submit" |
| Bid at the buyout price | "Bid ₱8,000 · wins now" | "Bid ₱8,000" (hides that it ends the item) |
| Submitting | "Placing ₱4,600…" | "Please wait" |
| Bid failed | "Your bid wasn't placed — someone bid ₱4,700 first. Next bid: ₱4,800." | "Error 409" |
| Off-step amount | "Bids go up in ₱100 steps. Try ₱4,600 or ₱4,700." | "Invalid amount" |
| Over the ceiling | "The most you can bid is ₱8,000 (the buyout price)." | "Amount too high" |
| Blocked | "You can't bid on this seller's auctions." | "You have been blocked for non-payment" |
| Timed setup | "Late bids extend the close by 30s, so nobody wins by sniping." | "Anti-snipe: ON" |
| Validation | "Buyout must be above the base price (₱4,000)." | "Invalid value" |
| Messenger link | "Winners use this to message you. It starts with m.me/" | "Enter URL" |
| Close all | "Close all now? 6 items, 4 have bids → 4 winners. The 2 items with no bids end unsold. Bidders are told the auction closed early." | "Are you sure?" |
| Irreversible | "Closing can't be undone." | "Warning! This action is permanent!!" |
| Safe choice | "Keep bidding open" | "Cancel" |
| Close one item | "Close now · Marco R. wins ₱4,600" | "End auction" |
| Block | "Block from my auctions — They can't place new bids on your auctions. You can unblock anytime." | "Report bogus buyer" / "Ban user" |
| Trust signal | "2 unpaid wins · 90d" | "⚠ Bogus buyer" / "Unreliable" |
| Mark unpaid note | "The winner isn't labelled or notified of a rating." | — |
| Share | "Batch 0416 is live. Post the link where your buyers are." | "Congrats on your launch!" |
| Empty blocklist | "No one is blocked. If someone keeps winning and not paying, tap ⋯ next to their bid and choose Block from my auctions." | "Nothing here yet!" |
| Facebook caption house rule | "No 'mine/steal' in comments — bids on BidBoard only." | — |

Sellers and buyers get the same voice. Names: sellers see bidders' full names; buyers see shortened names (e.g. "Marco R.", "J***"; exact rule OQ-12) and themselves as "You".

## Component Patterns

Behavioural rules. Visual specs live in `DESIGN.md` → Components.

| Component | Use | Behaviour |
|---|---|---|
| Bid action bar (`{components.bid-action-bar}`) | Item live bidding | Fixed to the bottom; always visible while the item is biddable. Row 1 stepper, row 2 Bid + Buyout side by side. Removed when the item closes. Any disabled control shows its reason in text above row 2. Replaced by the blocked notice for a blocked buyer. |
| Bid stepper (`{components.bid-stepper-button}`, `{components.bid-amount}`) | Action bar | Starts at the next valid bid. `+`/`−` move by the listing increment; `−` is disabled at the next valid bid, `+` at the ceiling. Tapping the amount allows typing a custom amount (see Interaction Primitives). If a new bid overtakes the selected amount, the amount resets to the new next valid bid and that is announced [ASSUMPTION]. |
| Bid button (`{components.button-bid}`) | Action bar | Label always mirrors the stepper amount. One tap places the bid — no confirmation sheet for ordinary bids [ASSUMPTION]. Signed-out tap opens the Sign-in sheet first. While submitting it reads "Placing ₱4,600…" and the whole bar is locked. When the selected amount equals the buyout price the label reads "Bid ₱8,000 · wins now", because that bid ends the item like Buyout. |
| Buyout button (`{components.button-buyout}`) | Action bar | Opens the Buyout confirmation sheet. Absent if the listing has no buyout (Bid then spans the row). Also steps aside while the buyer's selected amount equals the buyout price, since "Bid ₱8,000 · wins now" does the same thing [ASSUMPTION]. A bid at the buyout price ends the item, so Buyout never outlives it. |
| Price block + tick (`{components.price-block}`, `{components.price-tick}`) | Item page; seller item view | Updates live. Each new bid swaps the figure and shows `▲ +(increment)` with a `{motion.flash-duration}` flash, at most once per `{motion.flash-min-interval}`. With no bids it shows "STARTING BID" and the base price. |
| LIVE pill + countdown (`{components.live-pill}`, `{components.countdown}`) | Timed items, batch header, dashboard rows | Counts down every second. Jumps forward when an anti-snipe extension lands on that item. In a batch each item has its own close; the batch header shows the scheduled close. |
| Reconnecting line (`{components.reconnecting-line}`) | Item page, control room | Replaces the countdown while the live stream is down. |
| Open status (`{components.open-status}`) | Open-ended items | Replaces pill and clock. Shows age ("started 2d ago") on the seller side. |
| Status banner (`{components.status-banner}`) | Item page | Exactly one at a time, between the clock strip and the price block, by this precedence: **1 Bid failed > 2 Outbid > 3 Extended > 4 Winning > 5 Open**. A higher banner replaces a lower one; when it ends, the highest still-true banner returns. Bid failed lasts until the next bid attempt or 10s. Extended lasts `{motion.extended-banner-lifetime}` or until the next bid on that item, whichever comes first. Outbid, Winning and Open last while true. A new banner never takes focus. |
| Ladder (`{components.ladder-row}`) | Item page; seller item ladder | Newest first; leader row marked. Buyers see shortened names, sellers full names. Seller rows carry ⋯. **Pause ladder** (`{components.ladder-pause}`) freezes the ladder and stats so rows stop moving under a reader; price, clock and action bar stay live; the control then reads "N new bids · Resume" [ASSUMPTION]. |
| Stats grid (`{components.stats-grid}`) | Item page, control room, dashboard, results, sheets | Read-only. Values update live. |
| Control-room item row (`{components.item-row}`) | Control room | Sorted by most recent activity [ASSUMPTION]. A new bid flashes the row (`item-row-flash`). No bids shows "No bids yet". Tap opens the Item ladder (laptop: selects the row, keeps focus on it, fills the detail panel and announces "Showing ladder for Seiko 5" politely). |
| Trust signal (`{components.trust-signal}`) | Seller ladder, control room, blocked list | Shown beside a bidder's name only when they have ≥1 unpaid win in the last 90 days [ASSUMPTION]. Seller-only. Not tappable. |
| ⋯ menu (`{components.overflow-menu}`) | Seller ladder rows | **Block from my auctions** (and Unblock when already blocked). No profile or Messenger actions. |
| Confirmation sheet (`{components.confirmation-sheet}`) | Close all; Buyout | States the outcome in numbers before committing. Safe option always visible. Dismissing (scrim tap, swipe down, Esc) = safe option [ASSUMPTION]. Focus is trapped and returns to the trigger on close. |
| Sign-in sheet (`{components.sign-in-sheet}`) | Item page | Opens on the first Bid or Buyout tap while signed out. **Continue with Facebook** runs Facebook Login; **Not now** returns to the item with the amount kept. |
| Win panel (`{components.win-panel}`) | Win screen | "You won." · item · amount · "Message Liza to arrange payment." · **Message Liza on Messenger** opens the seller's `m.me` link (Messenger app if installed, otherwise messenger.com). |
| Status chip (`{components.status-chip}`) | Results | Awaiting payment → Paid or Unpaid via Mark paid / Mark unpaid. No bids is terminal. |
| Results winner card (`{components.results-winner-card}`) | Results | Shows the winner's full name so the seller can match an incoming Messenger chat. Mark paid / Mark unpaid on Awaiting payment; Offer to next bidder appears once Unpaid. |
| Batch lot row (`{components.batch-lot-row}`) | Buyer batch page | Status is personal: Live, Outbid, You lead, Open, Won, Sold. An extended lot shows its own countdown [ASSUMPTION]. Tap opens the item. |
| My bids row (`{components.my-bids-row}`) | My bids | One row per item bid on, newest activity first; tap opens the item (or the Win screen when won). |
| Dashboard auction row (`{components.dashboard-auction-row}`) | Dashboard | Tap opens the control room (live), Open-ended item control (open), Create (draft) or Results (ended). |
| Segmented control (`{components.segmented-control}`) | Dashboard | Live (n) · Scheduled (n) · Ended · Drafts. Exposed as a tab list with the selected segment marked. |
| Tab bar (`{components.tab-bar}`) / Desktop nav (`{components.desktop-nav}`) | Seller | Switches top-level surface; the active item is exposed as selected/current. |
| Item form controls (`input`, `toggle`, `increment-chip`, `option-card`, `step-progress`) | Create; Seller profile | Inline validation on blur and on save, naming the exact fix. Buyout is an optional toggle (exposed as a switch with its label); when on, it must exceed base. Increment is a one-tap chip (₱50 / ₱100 / ₱500; custom increments OQ-15). Option cards behave as a radio group. Step progress is read-only. |
| Photo slot (`photo-slot`) | Create step 2 | Tap an empty slot to add; each photo has a menu with **Make cover**, **Move left**, **Move right**, **Remove** (single-pointer alternative to any drag reordering). Upload shows progress; a failed upload shows "Photo didn't upload. Tap to retry." |
| Share caption | Share | Pre-filled with link, close time, anti-snipe rule and house rule; editable; character count shown. Copy caption copies and shows "Copied ✓" (`{components.copy-feedback}`); Open Facebook hands off to the Facebook app or site. |
| Quiet / destructive buttons (`{components.button-quiet}`, `{components.button-destructive}`) | Rows, menus | Quiet buttons act immediately (low stakes, reversible). Destructive menu items act immediately only when reversible (Block); irreversible bulk actions go through a confirmation sheet. |
| Empty state (`{components.empty-state}`) | Empty lists; Unavailable link | Says what's missing and where the action lives; never a dead end. |
| Focus ring (`{components.focus-ring}`, `{components.focus-ring-inset}`) | Every interactive element | Always visible on keyboard/switch focus; inset on strips and clipped blocks. |

## State Patterns

| State | Surface | Treatment |
|---|---|---|
| Loading | Item page, batch page, control room | Static skeleton strips in the final layout; photo blocks use `{colors.photo-placeholder}`. Bid and Buyout are disabled, with "Loading the live price…" as the reason, until the live price has loaded — never let someone bid against a stale number. [ASSUMPTION] |
| Unavailable link | Batch page, item page | Deleted, cancelled or mistyped link, or removed seller: empty state "This auction isn't available. It may have been removed or the link may be wrong." An *ended* auction is not unavailable — it opens in its closed state. |
| Empty — dashboard | Dashboard | First-time seller; must point to **+ New auction** (copy and layout OQ-4). |
| Empty — no bids yet | Item page | Price block reads "STARTING BID" with the base price; ladder area says "No bids yet"; the stepper starts at the base price, which is the first valid bid. |
| Empty — control room row | Control room | "No bids yet" in the status column. |
| Empty — My bids | My bids | "You haven't bid on anything yet. Bids you place show up here." |
| Empty — blocked list | Blocked bidders | "No one is blocked" + where blocking happens (mocked). |
| Empty — no bids at close | Results | Item ends unsold with the **No bids** chip and its base price. |
| Validation error | Create step 2; Seller profile | `{components.input-error}` frame + message naming the exact fix ("Buyout must be above the base price (₱4,000)." / "Enter your Messenger link — it starts with m.me/"). Save and Publish are disabled with the reason "Fix 1 item to publish"; the list shows which item. |
| Photo upload | Create step 2 | Progress bar in the slot; failure shows "Photo didn't upload. Tap to retry." Publish waits for uploads. |
| Publish failed | Create · Batch items | "Couldn't publish. Nothing went live — try again." Draft is kept. |
| Messenger link missing | First Publish | Publish opens Seller profile first; the auction goes live only after a valid `m.me/` link is saved. |
| Custom amount invalid | Item page | Amount frame turns `{components.bid-amount-error}`, Bid is disabled, and the message under the stepper names the fix: off-step → "Bids go up in ₱100 steps. Try ₱4,600 or ₱4,700."; over ceiling with buyout → "The most you can bid is ₱8,000 (the buyout price)."; over ceiling without buyout → "The most you can bid right now is ₱45,000 — 10× the current bid."; below floor → "The lowest bid now is ₱4,600." |
| Submitting | Item page | After tapping Bid: label "Placing ₱4,600…", stepper, amount, Bid and Buyout locked so a double tap can't send twice; no optimistic ladder row. Resolves on the server's answer; no answer within 10s counts as a network failure [ASSUMPTION]. |
| Bid failed — outbid while submitting | Item page | `status-banner-failed`: "Your bid wasn't placed — someone bid ₱4,700 first. Next bid: ₱4,800." Price updates; stepper resets to the new next valid bid. |
| Bid failed — auction closed | Item page | "Your bid wasn't placed — bidding closed before it arrived." Moves to the closed state. |
| Bid failed — network | Item page | "Your bid didn't reach us. Check your connection and tap Bid ₱4,600 to try again." No automatic retry; the amount is kept and re-validated [ASSUMPTION]. |
| Disabled controls | Item page; Create; sheets | Disabled tokens (`{components.button-bid-disabled}` etc.) plus a reason in text: "Bid unavailable — reconnecting", "Bid unavailable — loading the live price", "Lower bid unavailable — ₱4,600 is the lowest valid bid", "Higher bid unavailable — ₱8,000 is the buyout price". The reason is also the control's accessible hint. |
| Offline / reconnecting | Item page, control room | `{components.reconnecting-line}` replaces the countdown, the flash freezes, and Bid/Buyout are disabled until the stream resumes and the price is re-confirmed. On reconnect, apply the latest state without replaying flashes for missed bids. [ASSUMPTION] (timeout before an error: OQ-5) |
| Outbid | Item page; batch lot row; email | `status-banner-outbid` with the next bid amount; lot row status "Outbid"; outbid email. |
| Winning | Item page; batch lot row | `status-banner-winning`; lot row "You lead"; ladder shows the buyer as "You". |
| Extended (anti-snipe) | Item page; all viewers | That item's clock jumps to the new close; `status-banner-extended` "Bid in the last 30s — closing extended to 00:30" for `{motion.extended-banner-lifetime}` or until the next bid, then the viewer's standing banner (Outbid / Winning) returns. Other items in the batch are not extended. |
| Buyout reached | Item page | A bid at the buyout price ends the item immediately, exactly like Buyout (see Closed — bought out). Any other viewer's open Buyout sheet closes with "Buyout is no longer available — this item just sold for ₱8,000." |
| Closed — won | Item page → Win screen; email | Action bar removed; Win screen "You won. Message Liza to arrange payment." with **Message Liza on Messenger**. |
| Closed — bought out | All viewers of that item | Via Buyout or a bid at the buyout price: the item closes immediately; the buyer gets the Win screen; everyone else sees "Sold" and the final price. |
| Closed — lost / sold | Item page; batch lot row | Action bar removed; lot row "Sold"; final price and winner (shortened) shown. |
| Closed early | All bidders | Email "auction closed early" (Close all). |
| Open-ended | Item page; seller item control | `● OPEN` + `status-banner-open` "Open — Liza closes this manually"; no clock; no anti-snipe. Seller sees **Close & award**. |
| Blocked | Buyer on a blocking seller's auction | The server rejects the bid with a neutral error (never accept-and-discard, which would corrupt other viewers' ladders). The buyer sees only "You can't bid on this seller's auctions." — no reason, no mention of a list — in place of the action bar, from the moment a signed-in blocked buyer opens a biddable item and after any rejected attempt [ASSUMPTION — timing]. Browsing stays open. |
| Seller on own item | Seller signed in as a buyer | "You can't bid on your own auction." in place of the action bar [ASSUMPTION]. |
| Sign-in required at first bid | Item page | Browsing is open; the first Bid or Buyout tap opens the Sign-in sheet. After sign-in, return to the same item with the chosen amount still selected and re-validated against the current next valid bid before placing. |
| Sign-in cancelled | Item page | Closing the Facebook dialog or tapping Not now: "Sign-in cancelled. Your bid wasn't placed." The amount stays selected. |
| Sign-in — no email on Facebook | After sign-in | Facebook accounts registered by phone have no email. Ask once: "Add an email to get outbid and win alerts." with Skip; if skipped, My bids is the only place status shows [ASSUMPTION]. |
| Signed out | My bids | My bids has a **Sign out** link at the bottom [ASSUMPTION]. |
| Scheduled | Dashboard "Next up" | Row shows "Starts Fri 8:00 PM". Buyer view of a not-yet-started auction is open (OQ-6). |
| Light / dark | Everywhere | Both themes fully specified via `-dark` tokens in `DESIGN.md`. |

## Interaction Primitives

- **Bid stepper** — the primary path. One tap on `+`/`−` changes the amount by the listing increment; the Bid label updates instantly. The floor is the next valid bid: **the base price when there are no bids**, otherwise current bid + increment. The top is the ceiling (below).
- **Custom amount** — tap the amount to type. A custom amount must sit on the bid ladder: **current bid + n × increment** (n ≥ 1), or **base + n × increment** (n ≥ 0) when there are no bids. The **ceiling** is the buyout price, or **10× the current bid** when the listing has no buyout (10× the base price when there are no bids yet [ASSUMPTION]). Off-step, over-ceiling or below-floor amounts keep Bid disabled with the inline message in State Patterns. An amount **at** the buyout price is valid and wins outright (see Buyout); the stepper's `+` is disabled there with "Higher bid unavailable — ₱8,000 is the buyout price". The stepper remains the default; custom entry is secondary.
- **Bid** — one tap, no confirm [ASSUMPTION]. The outcome shows in place: ladder row, price flash, Winning banner, polite "Bid ₱4,600 placed. You're winning."
- **Buyout** — Buyout opens a confirmation sheet: "Buy Seiko 5 SNK809 now for ₱8,000? Bidding on this item ends and you win it." with **Buyout ₱8,000** and a safe **Keep bidding**. On confirm, **that item ends immediately and the buyer wins** at the buyout price; other items in a batch are unaffected. **A bid equal to the buyout price ends the item immediately, exactly like Buyout** — same win, same Win screen, same "Sold" for everyone else — so no bid can ever follow it. Whether reached by the stepper or a typed amount, the Bid button then reads "Bid ₱8,000 · wins now" and Buyout steps aside [ASSUMPTION — layout]. It is still one tap with no sheet, like any bid [ASSUMPTION]; the label carries the consequence.
- **Anti-snipe (timed only)** — if a bid lands in the last 30 seconds, **that item's** close moves to **bid time + 30s**. There is **no cap** on extensions; each late bid can extend again. In a batch, only the item bid on is extended; items close independently. Every viewer of that item sees the clock jump and the Extended banner.
- **Close now (seller, one item)** — the button names the outcome ("Close now · Marco R. wins ₱4,600"). Ends just that item. Confirmation and email: OQ-13.
- **Close all (seller, batch)** — always through the Close-all sheet: items still open, winners, unsold, total; "Closing can't be undone."; then **Close all 6 items** or **Keep bidding open**. Bidders are emailed that the auction closed early; winners also get the won email [ASSUMPTION].
- **Close & award (seller, open-ended)** — the only end for an open-ended item; names winner and amount on the button.
- **Block** — ⋯ on any ladder row → **Block from my auctions**. Immediate, per-seller: the person can't place new bids on any of this seller's auctions and sees only "You can't bid on this seller's auctions." Reversible via **Unblock** on the Blocked bidders list (one tap).
- **Mark paid / Mark unpaid** — one tap each on an Awaiting payment result. Mark unpaid adds to the winner's 90-day unpaid count and unlocks **Offer to next bidder** at that bidder's last bid [ASSUMPTION].
- **Copy** — link and caption copy in one tap each, confirmed by "Copied ✓".
- **Pause ladder** — one tap freezes the ladder and stats; one tap resumes [ASSUMPTION].
- **Banned:** confirmation dialogs on ordinary bids; auto-bidding or proxy bids [ASSUMPTION]; hover-only actions; drag-only actions; modal stacks deeper than one; easing or bouncing on price changes.

## Accessibility Floor

Behavioural. Contrast values live in `DESIGN.md` → Colors (all text pairs ≥ 4.5:1 in both themes after the live-red change; control boundaries via `{colors.border-strong}` and focus ≥ 3:1).

- **WCAG 2.2 AA** across buyer and seller surfaces, both themes.
- **Flutter web semantics on at startup:** call `SemanticsBinding.instance.ensureSemantics()` at app start on web so screen-reader users never have to find Flutter's hidden "Enable accessibility" placeholder. Every interactive widget has an explicit `Semantics` label, role and state. Release gate: the bid flow is tested with VoiceOver in iOS Safari and in the Facebook in-app browser, and TalkBack in Chrome.
- **Targets ≥ 44px** (`{spacing.target-min}`); primary bars and the stepper 48px (`{spacing.target-primary}`). Ladder rows are information; their only tap target is the 44px ⋯.
- **Timing (2.2.1 / 2.2.2):** timed auctions use the WCAG 2.2.1 **real-time exception** — the close is part of a live auction and cannot be extended for one user without changing it for everyone. Open-ended items have no time limit. Warnings are announced at **5:00, 1:00, 0:30 and 0:10** left, then "Bidding closed". Each anti-snipe extension is announced once to every viewer of that item. Any deadline on Offer to next bidder is not real-time, so it must be ≥ 20 hours or extendable (OQ-1). Pause ladder covers the auto-updating ladder.
- **Live regions (4.1.3):** announcements never move focus. In Flutter use `SemanticsService.announce` with the stated assertiveness, or `Semantics(liveRegion: true)`.

  | Event | Priority | Announcement |
  |---|---|---|
  | Outbid | assertive, once | "You've been outbid. Next bid: ₱4,600." |
  | Bid placed | polite | "Bid ₱4,600 placed. You're winning." |
  | Bid failed / rejected | assertive | "Your bid wasn't placed. Price is now ₱4,700. Your amount reset to ₱4,800." (or the closed / network reason) |
  | Stepper reset without a bid attempt | polite | "Next bid changed to ₱4,800." |
  | Stepper +/− | polite | "₱4,700" (the new amount) |
  | Other people's bids | polite, throttled | At most **one per 5 seconds**, summarising: "3 new bids. Price ₱5,100." Suppressed while the buyer is editing the amount or a sheet is open. |
  | Extended | polite, once per extension | "Closing extended. 30 seconds left." |
  | Countdown thresholds | polite (0:10 assertive) | "5 minutes left" · "1 minute left" · "30 seconds left" · "10 seconds left" |
  | Closed / won | assertive | "Bidding closed. You won at ₱4,900." / "Bidding closed. Sold to another bidder at ₱4,900." |
  | Bought out | assertive | "You bought Seiko 5 SNK809 for ₱8,000." |
  | Reconnecting / reconnected | polite, once each | "Reconnecting. Bidding paused." / "Reconnected." |
  | Blocked | polite | "You can't bid on this seller's auctions." |
  | Control room | polite, throttled 5s | Latest change only; never each row flash. |

- **Focus (2.4.7, 2.4.11):** visible `{components.focus-ring}` with its mandatory 2px offset everywhere; `{components.focus-ring-inset}` on full-width rows and clipped blocks. The fixed action bar and tab bar must never obscure the focused element: scroll views carry bottom padding equal to the bar height, and focus changes call `Scrollable.ensureVisible` so the element sits fully above the bar. Sheets trap focus and return it to the trigger on close, scrolled into view; Esc dismisses to the safe option.
- **Focus order (item page):** header → title → **Skip to bid** → clock → banner → price → stats → ladder (with ⋯) → action bar (−, amount, +, Bid, Buyout). Skip to bid jumps straight to the amount.
- **Screen-reader labels:** stepper buttons "Raise bid by ₱100" / "Lower bid by ₱100"; disabled `−` "Lower bid, unavailable. ₱4,600 is the lowest valid bid."; the amount is a labelled editable field "Your bid, ₱4,600" whose error is attached as its hint; Bid and Buyout are buttons whose names include the amount; ⋯ "More actions for J. Cruz". Masked names read as "Bidder J, name hidden" while keeping the visual "J***"; the buyer's own row reads "You (Marco R.)". Times and stats get spoken forms: "1 minute ago", "Increment ₱100", "2 unpaid wins in the last 90 days", "2 minutes 14 seconds left" (the countdown's label does not update every second). Toggles are switches with a checked state; segments, tabs and the desktop nav expose a selected state.
- **Reduced motion:** honour `MediaQuery.disableAnimations` (and `prefers-reduced-motion` on web): no `▲` colour flash, no row flash, no spinner rotation, no count-up. Figures change instantly; the `▲ +100` text still appears. Without reduced motion, flashes are still limited to one per second per element (2.3.1).
- **Colour is never the only signal:** LIVE has a word, Open has "● OPEN", every chip has a word, banners lead with a bold phrase, the leader row is bold as well as edged, the flash pairs with `▲`, toggles show knob position and ✓, selected segments and tabs carry an ink outline or bar, and disabled controls carry a reason in text.
- **Sign-in (3.3.8):** Facebook Login runs in the platform auth session (`ASWebAuthenticationSession` / Custom Tabs, or a full-page redirect on web), never an embedded WebView, so password managers and paste work. No app-side CAPTCHA or puzzle. Sessions last long enough that nobody is asked to sign in again mid-auction. The sheet title and button have accessible names; errors are described in text.
- **Text scaling (1.4.4, 1.4.10):** respect `MediaQuery.textScaler` without clamping and verify 200% browser zoom on Flutter web. Amounts and labels never truncate at 200%: control-room columns are intrinsic and stack, the stats grid goes 2 × 2 at ≥1.5×, and the action bar becomes sticky at the end of content at ≥1.5× or in a viewport under 500px high.

## Responsive & Platform

| Context | Behaviour |
|---|---|
| Phone browser (primary, buyers and sellers) | Single column, layouts as mocked at ~390px. Buyer has no tab bar; seller has the 4-tab bar. Includes the Facebook in-app browser. |
| Laptop browser (sellers) | Control room becomes master–detail: left nav (`{spacing.desktop-nav}`), item table, selected item ladder (`{spacing.desktop-detail}`). Close all sits top-right of the batch header; Close now in the detail panel. Breakpoint value open (OQ-7). |
| Laptop browser (buyers) | Not mocked (OQ-8). |
| Native app (optional) | Same Flutter surfaces. Never required for buyers. |
| Flutter web | Semantics enabled at startup; fonts bundled as assets (see DESIGN.md → Typography). |
| Light / dark | Both required and fully tokenised. How the mode is chosen is open (OQ-9). |
| Sign-in | Facebook only, via the platform auth session. Google deferred. |
| Notifications | Email now; optional web push later. |
| Facebook | Sign-in, link sharing, and opening the seller's `m.me` link only. |

## Trust & Bogus Buyers

The comment-thread problem that BidBoard inherits: people "win" and never pay. The response is split into a **private tool** and a **neutral fact**.

1. **Per-seller blocklist.** Any seller can block a bidder from their own auctions (⋯ → Block from my auctions). Only that seller sees the list; the blocked person only ever sees "You can't bid on this seller's auctions." and isn't labelled anywhere else. Unblock is one tap from **Blocked bidders**.
2. **Neutral computed signal.** When a seller marks a winner unpaid, BidBoard counts it. Sellers see the result as a grey, factual chip — "2 unpaid wins · 90d" — next to that bidder's name in ladders, control-room rows and the blocked list. It is computed only from BidBoard records, over the last 90 days. The winner isn't labelled or notified of a rating.
3. **No shared accusation list.** No cross-seller "bogus buyers" list, no reporting, no ratings — a count of what happened on BidBoard is something the product can stand behind; an accusation list is not.

## Notifications

Email, sent to the address on the buyer's Facebook profile (or the one they added if Facebook had none). Optional web push later.

| Trigger | Recipient | Content |
|---|---|---|
| Outbid | The bidder who lost the lead | "You've been outbid. Next bid: ₱4,600." + link back to the item |
| Won (incl. buyout) | The winning bidder | "You won Seiko 5 SNK809 at ₱4,900. Message Liza to arrange payment." + **Message Liza on Messenger** (opens her `m.me` link) + link to the Win screen |
| Auction closed early | All bidders on the batch | That the seller closed the auction early, and the bidder's outcome [ASSUMPTION] |

Frequency rules for outbid emails during fast bidding wars are open (OQ-10). Seller-side notifications are not decided (OQ-11). How the next bidder is told about an offer is open (OQ-1).

## Key Flows

Protagonists are illustrative, not researched: **Liza** [ASSUMPTION] runs *Liza's Timepieces*, a pre-loved watches & collectibles page, mostly from her phone and sometimes a laptop. **Marco** [ASSUMPTION] browses Facebook groups on his phone during his commute and arrives via shared links.

### Flow 1 — Liza publishes her first batch (Friday 16 Oct, afternoon, phone)

1. Liza signs in with Facebook for the first time and lands on an empty **Dashboard** that points her to **+ New auction**.
2. She taps **+ New auction** → **Create · Step 1 Format**: chooses **Batch** and **Timed**, ends Fri 16 Oct at 9:00 PM. The line "Late bids extend the close by 30s, so nobody wins by sniping." tells her the rule once.
3. **Next: add items** → **Create · Step 2 Item form**. For the G-Shock she types a buyout of ₱3,500; the field turns red: "Buyout must be above the base price (₱4,000)." She removes the buyout and taps **Save item**.
4. **Create · Batch items** shows six items with base · increment · buyout each. She taps **Publish · 6 items**.
5. Because it's her first publish, **Seller profile** opens: shop name "Liza's Timepieces", link `bidboard.app/liza`, and her Messenger link. She pastes her Facebook page URL; the field says "Enter your Messenger link — it starts with m.me/". She enters `m.me/lizastimepieces` and taps **Save and publish**.
6. **Share**: "Batch 0416 is live." She taps **Copy caption** ("Copied ✓"; link, close time, 30s rule and "bids on BidBoard only" are already in it), then **Open Facebook**, pastes and posts in her group.
7. **Climax:** she taps **Go to control room** and, seconds later, the first row flashes green with `▲ +100` — the thread that used to need her to scroll 300 comments is now one row per watch.

Failure: Publish is disabled while any item has a validation error or a photo is still uploading; the item list shows which one. If publishing itself fails: "Couldn't publish. Nothing went live — try again." and the draft is kept.

### Flow 2 — Marco retakes the lead in the final seconds (Friday 16 Oct, just before the 9:00 PM close, phone browser)

1. Marco taps Liza's link in the Facebook group. The **Batch page** opens in the Facebook in-app browser — no install. Six lots, closing 9:00 PM, LIVE 02:14.
2. He taps **Seiko 5 SNK809** → **Item live bidding**: ₱4,500, Next ₱4,600, Buyout ₱8,000, ladder below.
3. He taps **Bid ₱4,600**. The **Sign-in sheet** opens; he continues with Facebook and returns to the same item with ₱4,600 still selected and re-checked against the current price. "Placing ₱4,600…", then the bid lands: Winning banner, his row on top as "You".
4. At 00:48 J*** bids ₱4,700. Marco's screen shows **"You've been outbid — bid ₱4,800 to retake the lead"**, the stepper resets to ₱4,800, and an outbid email is sent.
5. At 00:12 he taps **+** once (₱4,900) and **Bid ₱4,900**.
6. **Climax:** his bid lands inside the last 30 seconds. The Seiko's clock jumps to **00:30** — the other five lots still close at 9:00 PM as scheduled. The amber banner "Bid in the last 30s — closing extended to 00:30" shows on every Seiko viewer's screen for 10 seconds, then Marco's Winning banner returns. Nobody bids again; the Seiko closes about 30 seconds after 9:00 PM.
7. The action bar disappears; Marco sees the **Win screen** — "You won. Message Liza to arrange payment." — and taps **Message Liza on Messenger**, which opens a chat with `m.me/lizastimepieces`. The won email carries the same button.
8. Later he opens **My bids** from the email: Seiko "Won", Orient Bambino v2 "Sold" (someone else won it).

Alternative (buyout): on the **Tissot PRX** in the same batch, Marco would rather not fight. He taps **Buyout ₱9,500**; the **Buyout confirmation sheet** says "Bidding on this item ends and you win it." with **Keep bidding** beneath. He confirms; the Tissot ends at once, every other viewer sees "Sold", and Marco lands on the **Win screen**.

Failure: his connection drops at 00:20 — "↻ Reconnecting…" replaces the clock and Bid is disabled ("Bid unavailable — reconnecting") until the price is re-confirmed, so he never bids against a stale number [ASSUMPTION]. If a bid is sent but the server never answers: "Your bid didn't reach us. Check your connection and tap Bid ₱4,900 to try again."

### Flow 3 — Liza closes a batch early and matches winners (Saturday 17 Oct, 8:55 PM, laptop)

1. Saturday's **Batch 0417** (six items, scheduled to close at 10:00 PM) is live. Liza opens the **Live control room** on her laptop: item table centre, the Casio F-91W's **Item ladder** on the right.
2. She notices **J. Cruz** leading the Casio F-91W with the grey chip "2 unpaid wins · 90d". She decides to let the bid stand for now.
3. She needs to leave, so she clicks **Close all**. The **Close-all sheet** says: 6 items, 4 have bids → 4 winners, 2 unsold, total ₱12,550. "Closing can't be undone."
4. She clicks **Close all 6 items**. Every bidder is emailed that the auction closed early; winners also get the won email with her Messenger button.
5. **Climax:** **Results** opens: four winner cards with **Awaiting payment** chips and full names, two **No bids**. Within minutes Messenger chats arrive from winners — each one's Win screen told them to message her — and she matches each chat to a card by name, item and amount. No hunting through comments for who won what.
6. When Ben pays, she clicks **Mark paid** and his chip turns **Paid**.

Alternative (open-ended single item): on **Swatch Irony Chrono** — no clock, "● OPEN" — she taps **Close & award · Ana P. ₱900** from the **Open-ended item control** when she's happy with the price.

Failure: if closing fails, the sheet stays open with "Couldn't close the auction. Nothing changed — bidding is still open. Try again." If a winner never messages, Flow 4 picks it up.

### Flow 4 — Liza handles an unpaid winner and blocks him (Monday 19 Oct, phone)

1. In **Results**, J. Cruz still hasn't messaged or paid for the Casio F-91W. Liza taps **Mark unpaid**. The chip turns **Unpaid**; his count becomes part of the neutral signal. He is not notified.
2. **Climax:** **Offer to next bidder · Ana P. ₱600** appears. She taps it. Ana is told about the offer (how, and how she accepts — OQ-1); once she accepts, she gets the same Win screen, "Message Liza to arrange payment." The item is rescued without restarting the auction.
3. Next week J. Cruz bids on another of her items. In the **Item ladder** she sees "3 unpaid wins · 90d", taps **⋯** → **Block from my auctions**. From then on he sees only "You can't bid on this seller's auctions."
4. In **Blocked bidders** she sees him listed with when and where she blocked him. If she changes her mind, **Unblock** is one tap. With nobody blocked, the empty state explains where blocking happens.

Failure: if Ana declines or doesn't respond, the Casio stays **Unpaid** and unsold; any offer deadline must be ≥ 20 hours or extendable (OQ-1).

## Open Questions

| # | Question | Why it matters |
|---|---|---|
| OQ-1 | Offer to next bidder: how is the next bidder notified (email?), how do they accept or decline, and is there a time limit (must be ≥ 20 hours or extendable)? | Not covered by the decided notification set; Flow 4 depends on it. |
| OQ-2 | Where does My bids live in buyer navigation (buyers have no tab bar)? What exactly does it list? | Surface is decided; entry point and contents are not. |
| OQ-3 | Account (seller) contents beyond Seller profile — and is Seller profile a section of Account or its own screen? | Tab exists in the mockups with no defined screen. |
| OQ-4 | Dashboard empty state (first-time seller) copy and layout. | Not mocked; Flow 1 starts here. |
| OQ-5 | Reconnecting behaviour for the live stream — confirm the proposed treatment and the timeout before showing an error. | Reliability is a brand pillar; the treatment above is an [ASSUMPTION]. |
| OQ-6 | What buyers see on a scheduled auction before it starts. | Links may be shared before start time. |
| OQ-7 | Laptop breakpoint for the master–detail control room. | Mockup shows ~1100px only. |
| OQ-8 | Buyer layout on laptop browsers. | Buyers can arrive from any device that opens Facebook. |
| OQ-9 | Light/dark selection: follow system setting, manual toggle, or both? | Both themes are required; the switch is undecided. |
| OQ-10 | Outbid email throttling during rapid bidding wars. | One email per outbid could flood inboxes. |
| OQ-11 | Seller-side notifications (new bid, auction ended, winner hasn't messaged). | Not decided. |
| OQ-12 | Buyer name-shortening rule — "Marco R." and "J***" both appear in mockups. | Privacy and consistency; must be enforced server-side before any buyer-facing ladder API ships. |
| OQ-13 | Does Close now on a single item (and Close & award) need a confirmation sheet, and does it trigger the "closed early" email? | Mockups show a direct button; Close all has a sheet. |
| OQ-14 | Currency: mockups use ₱ throughout — is the product single-currency? | Formatting; store ISO currency code + minor units from day one either way. |
| OQ-15 | Can sellers set a custom increment beyond ₱50 / ₱100 / ₱500? | Fixed chips are category-specific in a category-agnostic product. |
| OQ-16 | Editing and cancelling: which fields can change on a Draft, Scheduled or Live auction (e.g. buyout or increment once bids exist), and can a Scheduled or Live auction be cancelled? | Changes server-side bid validation mid-auction. |

# BKS CRM prototype v0.3

Usability and engineering update on top of v0.2, from a combined Senior Developer and UX/UI review on 25 October 2026. No new client scope: business rules, approval semantics and the confirmed TOR baseline are unchanged. v0.2 and v0.1 are preserved untouched.

## What changed

### Checker review (biggest change)
- Approving a revision no longer means reading raw JSON. The review dialog shows a **field / current / proposed** table with changed fields highlighted and listed first, plus a change count. Raw payload remains under "ดู Payload ทางเทคนิค (JSON)".
- `CRM.request()` snapshots the record being replaced (`before`) so the diff reflects what was live at submission time. Point adjustments show current → after balance; tier changes compare each level.
- Approvals page: pending / approved / returned / all filter with counts, pending first, submission time, request type, return reason, and read-only "ดูรายละเอียด" for decided requests.

### Navigation and readability
- Thai-first type: IBM Plex Sans Thai for body text (Prompt kept for headings); base 14px, no meta text under 11px (v0.2 used 9–10px in places).
- Every sidebar item has an SVG icon (v0.2: 5 icons, the rest "·"), an active marker, `aria-current`, and a live pending-approval count.
- Command palette: ⌘K / Ctrl+K (or the search button) jumps to any of 22 pages or starts Create campaign / group / reward, rule test, LIFF. Arrow keys + Enter.
- Overview: stat cards and "รายการที่ต้องดูแล" are clickable; quick-create tiles added.
- Dates display as Thai short dates (e.g. 1 ต.ค. 2569) in lists, dialogs, audit and the guided editor summary; storage stays ISO.
- Status badges cover every state (cancelled, scheduled, sent, processed were raw English) and use distinct good / warning / danger / info tones.

### Campaign lifecycle
- "พัก" now asks for confirmation and explains the effect; paused campaigns get "เปิดต่อ" (blocked if the end date has passed). Cancelled campaigns no longer show pause/cancel.

### LINE LIFF
- Shared `CRM.eligibility()` drives both redeem and the UI: reward cards show "ขาดอีก X แต้ม" with a progress bar, or the actual blocking reason, before the member taps. Detail sheet offers booking instead of a redeem button when points are short.
- Coupons show days remaining (amber within 7 days), sort usable first by soonest expiry, and include the "อยู่ระหว่างชำระ" filter.
- SVG tab bar with `aria-current` and a usable-coupon count; larger labels. Point history uses Thai labels and timestamps. Home shows the best next reward rather than the first one.

### Engineering
- Own storage key `bks-tor-prototype-v03` and draft keys `bks-v03-draft:*` / `bks-v03-editor:*`; nothing in v0.3 reads or writes v0.2 data.
- Error toasts use `role="alert"` with a distinct style; success stays `role="status"`.
- Escaped the member ID in the console coupon dialog. Mobile sidebar closes on Esc or outside click and reports `aria-expanded`.
- Same no-build stack: `crm.css`, `builders.css`, `crm-store.js`, `builders.js`, `console.js`, `liff.js`.

## Run

From the parent `prototype` folder:

```sh
python3 -m http.server 8774 --bind 127.0.0.1
```

Open `/index.html`, `/v0.3/console.html` or `/v0.3/liff.html`.

## Limits

Unchanged from v0.2: all figures, routes, tiers, templates and financial effects are examples; nothing sends OTP, messages, payments or e-ticket requests. Resuming a paused campaign reuses its approved revision without a new checker request; confirm with BKS whether resume should require approval in production.

## Imported discount codes and coupon usage — added 2026-10-25

Requested change: the Console must not create or manage discounts itself. The **e-ticket system (or a partner) generates the discount codes** and their discount rules; the Console only **imports those codes into a specific Coupon** and hands them out when members redeem points. The CRM therefore needs no write-back or publish step into the legacy e-ticket system.

> Scope note: the confirmed scope (`../../outputs/BKS-CRM-Confirmed-Prototype-Scope-2026-10-25.md`, superseded item 7) excluded partner code inventory and timed activation. This section implements them on the client's 25 Oct request; update the scope record and confirm with BKS.

### Console
- Each reward has a **code issuer**: BKS e-ticket or a named partner. Coupon "value" is now the **face value** used for display and budget; the real discount is enforced by the issuer.
- **Import codes** (Rewards, Coupons dashboard, or ⌘K → นำเข้าโค้ดส่วนลด): pick the target coupon, upload `.csv/.txt` or paste. One code per line; optional 2nd column is the code's own expiry (`YYYY-MM-DD`); header rows are skipped.
- **Pre-import check** before anything is saved: importable / duplicate in file / already in the tenant (any coupon) / invalid format or already-expired. If importing everything could exceed the remaining budget, a warning is shown.
- **Code pool** per coupon: available / issued-unused / used / expired-void counts, batch history (file, who, when, skipped rows, earliest code expiry), and "void unissued codes" for a wrong batch. Codes are **masked** in the UI; the full code is only in the admin CSV export.
- Members cannot redeem when the pool is empty (`โค้ดหมด`). Each coupon's expiry is the earlier of "redeem date + validity days" and the code's own expiry from the issuer.
- Guided reward editor: issuer choice (+ partner name), a partner template, face value, redeem cap ("not more than imported codes"), and the code display window (5/15/30 min). The diff for checkers includes the issuer and the window.

### LINE LIFF — "ใช้คูปอง"
- An unused coupon shows its validity ("ใช้ได้อีก X วัน · ถึง …") and a **masked code**. Tapping **ใช้คูปอง** opens a confirmation that explains the coupon becomes used immediately and the code shows for the window (default 15 min, matching the earlier v0.1 flow).
- After confirming: the code, a live countdown, copy and open-e-ticket buttons, and "กดใช้เมื่อ …". When the window ends the card becomes **ใช้แล้ว · date/time** with the code masked and no copy button, so members always see whether a coupon is still unused.
- Partner coupons say to show the code at the partner instead of e-ticket.

### Budget counting
- **Budget used = value of coupons with status `used` only.** A coupon becomes `used` when the member taps ใช้คูปอง (or staff record it at the counter: "บันทึกใช้แทนสมาชิก"). No e-ticket confirmation is needed.
- **Remaining = budget − used.** Redeemed-but-unused coupons are shown separately as "แลกแล้ว รอใช้" and are **not** deducted. Expired unused coupons never touch the budget and do not refund points.
- Redemption stops when the pool is empty or the remaining budget is below one coupon's value.

### Insights and recommendations
1. **Why the code is hidden until "ใช้คูปอง":** without an e-ticket callback, the member's tap is the CRM's only usage signal. If the code were visible on redeem, members could use it without tapping, and the budget would under-count. Hiding it until activation keeps the "used" count trustworthy.
2. **What the CRM can't know:** a member can tap use but never pay, or copy the code and use it later. "Used" means *activated by the member*, not *settled at e-ticket*. Recommend a monthly **batch reconciliation file** (redeemed codes from e-ticket → import) rather than a real-time integration, if finance needs settled figures.
3. **Control exposure through the import.** Because outstanding coupons aren't deducted, the real cap is the number of codes imported. Ask the issuer for batches sized to the budget (stock × face value ≤ remaining budget); the import warning enforces this softly.
4. **Codes are bearer value.** Mask them in the UI, keep full codes only in admin exports, have the issuer bind each code to single use and an expiry, and void unissued codes on their side when a batch is voided here.
5. **Agree a file contract with the e-ticket vendor and partners:** column order, code format/charset, per-code expiry, batch reference, and how the discount amount maps to the coupon's face value. This is the only integration point and it is one-way.
6. **Production:** code assignment must be an atomic single-writer operation (no double assignment under concurrency), and the activation window should be enforced server-side; this prototype runs in localStorage only.

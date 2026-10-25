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

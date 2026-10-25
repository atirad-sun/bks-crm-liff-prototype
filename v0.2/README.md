# BKS CRM prototype v0.2

Guided admin creation update based on ui-ux-pro-max and the user-selected guided steps with live summary. Original requirements and latest business decisions remain in force; this is a usability update, not new client scope.

Open `console.html` and use Create campaign, Create group or Create reward. All three use dedicated step-by-step editors rather than long modals.

- Campaign: template, selectable tiers/routes/ticket types, date/day/time shortcuts, earning presets, stacking, advanced limits and review before checker approval.
- Audience: starter groups, multi-select filters, AND/OR, counts from sample members, optional purchase filters, Dynamic/Snapshot and review.
- Reward/coupon: value presets, catalogue imagery, redemption availability, price/validity/quota presets, budget suggestion and review before checker approval.
- All: local draft autosave/recovery, back/edit without losing data, clear required-field errors, keyboard-friendly native selections and responsive controls.

## Versions and state

v0.1 is preserved at `../v0.1/` with a recorded SHA-256 manifest under project outputs. v0.2 uses its own localStorage key `bks-tor-prototype-v02`. Draft keys also include tenant, demo user, editor type and record ID. No v0.2 edit changes v0.1 storage.

Serve from the parent `prototype` folder:

```sh
python3 -m http.server 8774 --bind 127.0.0.1
```

Then open `/v0.2/console.html`, `/v0.2/liff.html` or the parent version launcher.

## Limits

All figures, routes, tiers, templates and financial effects are examples. Nothing sends OTP, messages, payment or e-ticket requests. Audience profile filters evaluate against six sample members, not a connected customer database. Route/ticket rules remain dependent on actual vendor data. Pending legacy identity and integration facts remain pending.

New multi-select campaign conditions are retained by revisions and evaluated in rule preview. Reward conditions are shared with LIFF; unsupported source-dependent restrictions do not silently grant entitlement. Dynamic audiences are evaluated at compose time; dispatch still rechecks consent/restrictions. Production concurrency, isolation, security, provider receipts and rule evaluation require a backend.

The unchanged initial business scope remains documented in `../../outputs/BKS-CRM-Confirmed-Prototype-Scope-2026-10-25.md`.

## Campaign automation consolidation — implemented 2026-10-25

Following the approved UX evaluation, v0.2 now has one five-step Campaign setup: starter, eligibility/period, points/limits, optional automatic communication, and one review/checker request. Birthday-month eligibility is explicitly selectable independently of tier. Points still require qualifying purchases; neither birthday timing nor approval issues points or coupons. Communication supports selectable channels and message starters/shared templates, editable content, campaign-start or birthday-month timing, local send time, quiet hours and repeat limits. It defaults off, with source-dependent win-back/payment triggers unavailable. The prototype stores configuration and displays dependency/history states; it has no production scheduler or provider dispatch.

One campaign payload and revision includes both earning and communication settings. Domain checks run at request and approval; configured channels, message content, times and repeat limits are validated. Communications being disabled or unfunded does not alter the earning configuration. Rewards & Coupons remains separate. The sidebar no longer includes Automation; old #automations links show a transition page. Existing workflow records remain read-only on the Campaign surface and old standalone approvals cannot activate them. Fresh example state no longer creates standalone workflows. No name-based migration or auto-activation occurs. Four-step campaign drafts restore at the new review step with messaging off unless previously configured. Old birthday campaign eligibility is preserved. v0.1 remains unchanged.

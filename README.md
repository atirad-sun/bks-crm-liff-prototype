# Prototype versions

- **Latest v0.3:** [Console](v0.3/console.html), [LINE LIFF](v0.3/liff.html), [version notes](v0.3/README.md).
- **Preserved v0.2:** [Guided Console](v0.2/console.html), [LINE LIFF](v0.2/liff.html), [version notes](v0.2/README.md).
- **Preserved v0.1:** [Console](v0.1/console.html), [LINE LIFF](v0.1/liff.html), [original launcher](v0.1/index.html).
- `index.html` opens the latest version and links to v0.2 and v0.1. Root `console.html`/`liff.html` remain compatible v0.1 entry points.
- v0.2 and v0.3 each use independent demo storage. The v0.1 snapshot is preserved byte-for-byte and has an integrity manifest in project outputs.

# BKS CRM and LINE LIFF prototype

Updated 25 October 2026 from the original client TOR, workflow PDF and recorded user interview. See [confirmed scope](../outputs/BKS-CRM-Confirmed-Prototype-Scope-2026-10-25.md) for provenance, superseded proposals and outstanding BKS/vendor decisions.

- `index.html` — prototype launcher
- `console.html` — desktop Marketing Console
- `liff.html` — responsive LINE LIFF journey, presented without prototype or device chrome

All data and actions are simulated. Shared sample state persists in this browser's localStorage across Console and LIFF; use the Console footer's reset action to start over. There is no backend, genuine tenant isolation, real OTP, sending, payment or production integration. Do not enter real personal data or credentials. Demo National ID/phone entries are not retained. The current LIFF has no partner module.

The shared catalogue uses consistent sample pricing; these values are not approved BKS policies. Membership tiers, checker-pool self-approval, communication credit, source-readiness labels and coupon/budget dashboards reflect the latest interview decisions. Exact policies and source fields remain pending. Route restrictions on coupon redemption require actual e-ticket evidence; code use is external and still requires server-side validation/single-use integration in production.

Earlier UI exports are preserved in `archive/`; earlier discovery mockups elsewhere in the project are not the current entry points. The application uses `crm.css`, `crm-store.js`, `console.js` and `liff.js` without a frontend build step.

To preview locally from this directory:

```sh
python3 -m http.server 8774 --bind 127.0.0.1
```

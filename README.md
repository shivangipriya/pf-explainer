# PF Rejection Explainer

A single-page, fully offline tool that explains, in plain words, why an Indian Provident Fund claim gets rejected, what recently changed in the rules, and keeps a private record of your claim and every fix you attempt.

## What it does

- **Explains several documented rejection reasons** (not a complete list): bank/KYC not seeded or approved, Aadhaar mismatch in name/DOB/father's name, employer approval demanded on a transfer claim (removed for most cases w.e.f. 15 Jan 2025), wrong/outdated paper forms with attestation rules (explicitly scoped to the offline route), and eligibility rules (minimum balance and waiting period, changed by Board approval of 19 Nov 2026, effective on formal notification). Each reason pairs a plain-words explanation with what to do next, citing the official source it rests on, and states that correction and resubmission never guarantee approval.
- **Tracks your claim privately**: claim type, date, the exact rejection remark, status (open / resubmitted / raised on EPFiGMS / settled) and notes. Everything stays in the browser's local storage on the device; nothing is sent anywhere.
- **Lists the real next steps**: claim status on epfindia.gov.in, EPFiGMS grievance portal, CPGRAMS escalation, and EPFO's warning that its services are free and no agent is authorised to charge for them.

## What it deliberately does not do

- It cannot check your claim, contact EPFO, or file anything. Only the official portals can.
- It makes no settlement-time promises and presents no performance statistics: older EPFO pages mention timelines but are undated and conflict with newer notices, so the tool omits them rather than quote a number to plan around.
- It does not decode rejection remarks beyond what current official sources support; for anything unfamiliar it says so and points to EPFiGMS with the remark quoted word for word.

## Grounding

- PIB release of 19 Nov 2026 on the 240th CBT meeting (minimum balance for withdrawal approved to fall from 25% to 10%; waiting period for final withdrawal from 12 months to 2 months; a Board approval only: the PIB release says the scheme amendments will be notified after Ministry approval and gives no effective date, and no notification was found on 25 Nov 2026, so the page states it as pending, not in force; the 25% / 12-month baseline is attributed to PIB 19 Nov 2026, which calls 25% "existing", and to Lok Sabha unstarred question 3567 of 10 Aug 2026, which says EPFO has introduced the 12-month wait).
- PIB release of 16 Jun 2025 on EPFO reform measures (bank seeding without employer approval from April 2025; cancelled-cheque-leaf upload removed; transfer-claim employer approval removed w.e.f. 15 Jan 2025; Aadhaar-based online profile correction; official services are free).
- Form 19 general instructions, epfindia.gov.in (offline attestation rules, SMS claim-stage alerts, claim status on the website).
- EPFO Members FAQ, epfindia.gov.in - undated, used only where it does not conflict with dated sources, and labelled as such in the page.

All sources accessed 23 Nov 2026 and the claims re-checked against them on 25 Nov 2026 (PIB 16 Jun 2025: PRID 2136592; PIB 19 Nov 2026: PRID 2338667; Form 19 instructions on epfindia.gov.in).

## Engineering

- One `index.html`, no build step, no network calls after load, no dependencies.
- Mobile-first responsive layout, ARIA tab pattern, status announcements via `role="status"`.
- A blocked save never discards typed input: validation refuses the save, explains why, and leaves the form untouched.
- All stored user text is HTML-escaped on render; storage corruption falls back to an empty list instead of throwing.
- Tests: `pf-test.js` (Node, no dependencies) covers structure, grounding claims and exclusions, storage round-trips and corruption handling, the blocked-save path, and render escaping. Run with `node pf-test.js`.

Maintained by [Shivangi Priya](https://www.linkedin.com/in/shivangii-priyaa).

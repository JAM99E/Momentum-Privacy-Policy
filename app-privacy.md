# App Privacy questionnaire

Answers for App Store Connect → App Privacy. **Every answer below is derived
from the code**, not from what a habit tracker typically does — a six-lens audit
traced each field from the app to the wire to the database.

Getting this wrong is worse than getting it late: the answers appear on your
product page as the "Privacy Nutrition Label", and a mismatch between the label
and observed behaviour is a rejection *and* a trust problem.

---

## Does this app collect data? **Yes**

---

## Data types to declare

All of the following are **Linked to You** — every row is keyed by your Supabase
user id — and all are used for **App Functionality** only. None is used for
Analytics, Advertising, Product Personalisation, or Developer Advertising.

| Category | Type | Why it's collected |
|---|---|---|
| Contact Info | **Name** | From Sign in with Apple, shown in Settings |
| Contact Info | **Email Address** | From Sign in with Apple; may be a private-relay address |
| Identifiers | **User ID** | The Supabase account id every row hangs off |
| User Content | **Other User Content** | Habit names and units (free text), completion records |

### Tracking: **No**

Momentum does not link any data to third-party data for advertising or
measurement, and contains no advertising identifier, no analytics SDK, no crash
reporter, and no attribution framework. **Do not** enable App Tracking
Transparency — an ATT prompt in an app that does no tracking is itself a review
problem.

---

## Health & Fitness: **Not collected**

This is the answer most likely to be got wrong, so the reasoning is worth stating.

Momentum **reads** five HealthKit types — step count, walking/running distance,
exercise minutes, active energy, and workouts (date and coarse type only). It
never writes: authorisation is requested with an empty `toShare` set, and the
entitlement carries no clinical-records access.

**None of it leaves the device.** The readings are cached in a local SwiftData
table that appears in no wire type and no server table. Apple's questionnaire
asks what is *collected* — meaning transmitted off-device or stored by the
developer — and Health readings are neither.

**What does leave** is the conclusion: that you earned the badge for covering 100
km on foot, and the date. That is declared above as User Content, and the
privacy policy says so in plain words rather than claiming nothing
Health-related is transmitted.

> If a reviewer asks, the distinction is: no Health *data* is collected; some
> Health-*derived* facts are, and they are disclosed.

---

## Also not collected

Location · Contacts · Photos or Video · Audio · Browsing History · Search
History · Purchases · Financial Info · Sensitive Info · Diagnostics · Product
Interaction · Advertising Data · Device ID · Coarse Location

---

## The disclosure that matters most

Habit names are **free text**, stored on the server in plain readable form,
alongside the exact record of which days each was and was not completed.

A habit called "Take medication" plus its miss history is, in practice, sensitive
information about a person. It is not treated as a special category by the
questionnaire — it is User Content — but the privacy policy gives it its own
callout rather than a footnote, because a user deserves to know before they type
it.

Do not soften that section to make the label look tidier. It is the most
important true thing on the page.

---

## Third-party SDKs

One: **supabase-swift** (auth, database, storage client). It is a client library
for our own backend, not an analytics or advertising SDK, and collects nothing
on its own behalf.

Supabase acts as a **data processor** — it stores the data so the app can
function and does not use it for its own purposes. If you later add a
Data Processing Agreement or a DPO contact, this is the section to update.

---

## Account deletion

Required to be in-app for any app offering account creation, and it is:
**Settings → Delete Account**. It calls a server function that hard-deletes the
`auth.users` row; every user-owned table cascades from it.

Two honest caveats, both stated in the privacy policy:

1. **Local data is not removed** — deleting the account clears the server copy;
   the on-device copy goes when the app is deleted.
2. **Anonymous population counts do not change** — they contain no identifiers
   and there is nothing in them to delete.

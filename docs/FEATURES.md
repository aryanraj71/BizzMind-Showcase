# BizzMind: feature catalogue

[← Back to README](../README.md)

This page describes **what BizzMind does**, not how it is implemented. Status markers:

| Marker | Meaning |
| :---: | --- |
| ✅ | Built and covered by automated tests (run against generated demo data) |
| 🟡 | Partly built, or built but not yet verified in a real deployment or device |
| ⚪ | Designed only: the integration point exists, the capability is not built |

The product is split into two workspaces with a switch between them. **Insights** holds the
analytics and intelligence. **Sales** holds the CRM and the field. Each role gets its own
landing page.

---

## Contents

1. [Analytics & dashboards](#1-analytics--dashboards)
2. [Predictive intelligence](#2-predictive-intelligence)
3. [Decision intelligence](#3-decision-intelligence)
4. [Ask BizMind (AI Copilot)](#4-ask-bizmind-ai-copilot)
5. [Collections & receivables](#5-collections--receivables)
6. [CRM](#6-crm)
7. [Dealer–distributor CRM](#7-dealerdistributor-crm)
8. [Meetings & the accountant](#8-meetings--the-accountant)
9. [Data onboarding](#9-data-onboarding)
10. [Platform, security & compliance](#10-platform-security--compliance)
11. [Mobile app](#11-mobile-app)
12. [WhatsApp Customer 360 extension](#12-whatsapp-customer-360-extension)
13. [Landing page](#13-landing-page)

---

## 1. Analytics & dashboards

| Feature | Description | Status |
| --- | --- | :---: |
| Overview | Revenue, gross profit, margin, orders, average order value, receivables, inventory value and pipeline, each against the previous period, with drill-down | ✅ |
| Sales, customer, inventory, finance analytics | One module per area, with filters and click-to-filter charts | ✅ |
| Gap analysis | Where the business falls short of its own history and targets | ✅ |
| Customer 360 | Everything about one account on one page | ✅ |
| Dashboard suite | Day, Item, Payment, Gross-profit, Snapshot and Trend views | ✅ |
| Map View | Interactive globe drilling down to Indian states and districts. Addresses are matched to locations through a PIN-code directory, with fallback rules. Includes custom sales territories and a sales-team tree | ✅ |
| Boost | Customer movement between segments, a cohort matrix and saved reports | ✅ |
| Target View | Targets across several dimensions with achievement tracking and CSV export and re-import | ✅ |
| Liquidity | Current, quick and cash ratios and working capital, each with an explanation of its source. A missing liability is shown as unknown, never as zero | ✅ |
| Reports | A report library with export, limited to people who have export permission | ✅ |
| Master filter | One filter bar that applies across every screen | ✅ |

## 2. Predictive intelligence

Every prediction states its method and its limits on screen.

| Feature | Description | Status |
| --- | --- | :---: |
| Sales forecast | Holt-Winters exponential smoothing compared against a seasonal-naive baseline, with backtested error shown | ✅ |
| Customer-decline risk | An explainable score per account, listing its inputs | ✅ |
| Stock-out risk | Days of cover per product | ✅ |
| Anomaly detection | Unusual movements flagged for review | ✅ |
| Root-cause analysis | Breaks a change into contributing factors (customers, products, regions), never presented as proven causes | ✅ |
| 13-week cash view | Money in and out shown as a range, based on each customer's own payment history. Labelled as an estimate, never a bank balance | ✅ |
| Usually bought together / missed reorders | Cards that name the accounts involved and become call tasks for each account's salesperson | ✅ |

## 3. Decision intelligence

| Feature | Description | Status |
| --- | --- | :---: |
| Decision Center | Recommended actions with an owner and a due date | ✅ |
| What-if simulator | Change assumptions and compare the result with the baseline | ✅ |
| Tasks & alerts | Task views (due today, overdue, this week) and a notification bell with per-kind preferences | ✅ |
| Business memory | Records whether a decision was carried out, and what happened after | ✅ |
| Morning brief | The day's priorities for people who opted in. Off by default and queued, never sent automatically | ✅ |

## 4. Ask BizMind (AI Copilot)

| Feature | Description | Status |
| --- | --- | :---: |
| Plain-language questions | Matches a question to one of a fixed list of read-only analytics tools: sales summary, why a figure changed, who owes money, what's running out, promises, disputes, collections against target, cash, and more | ✅ |
| Safe by design | Free text never becomes a database query. The worst a hostile question can do is pick the wrong read-only report, and only one the user could already open | ✅ |
| Explicit refusal | A question it can't place is declined, with a list of what it *can* answer. It doesn't guess | ✅ |
| Works offline | Runs on deterministic routing and wording, with no API key and no network | ✅ |
| Live LLM wording | Interface designed. An LLM would only reword results that were already computed, with every number checked against the input | ⚪ |

## 5. Collections & receivables

| Feature | Description | Status |
| --- | --- | :---: |
| Follow-up | Reminder templates, schedule rules in the company's own time zone, a WhatsApp send queue, statements and history | ✅ |
| Public statement page | A lightweight page a customer opens from WhatsApp, with a UPI payment link | ✅ |
| Customer replies | *I have paid* / *I will pay by…* / *This bill is wrong*, sent from the page. A claim of payment is never treated as a payment | ✅ |
| Promise to pay | Recorded by a person, checked against the books, and pauses chasing while it stands | ✅ |
| Dispute register | Per bill, not per customer, so undisputed bills are still followed up | ✅ |
| Collection ownership | A salesperson can chase their own accounts without seeing the whole company's receivables | ✅ |
| Chase effectiveness | Whether payments followed the messages. Presented as a correlation, with each payment credited to at most one message | ✅ |
| Credit control | Request, approve or reject with a reason. Two different people must be involved. The system records the decision; it never changes a credit limit itself | ✅ |
| Payment links | "Pay bill N: ₹X" links through a payment-gateway integration (Razorpay) when the deployment configures it. "Paid" waits until the books confirm it | 🟡 |

## 6. CRM

| Feature | Description | Status |
| --- | --- | :---: |
| Leads & pipeline | Lead list and drag-and-drop board with count, total and weighted value per stage. Each company sets its own stages, sources and lost reasons. Leads quiet for a week are flagged | ✅ |
| Enquiry intake | Shareable enquiry page with QR code, website form and webhook, plus round-robin assignment rules and a consent-gated thank-you message | ✅ |
| Lead → customer | Converts a won lead and carries its history over, with duplicate checks and lead merging | ✅ |
| Customer record | Edit, add, set owners, contacts, notes, search, tags and custom fields | ✅ |
| Timeline | Calls, visits, files and events with a customer in one timeline. Each reader sees only what their modules allow | ✅ |
| Tickets | Customer issues with categories, priorities, due times and escalation | ✅ |
| Quotations | Numbered by financial year, with CGST/SGST or IGST calculated, sent as a PDF, and accepted on the customer's own page | ✅ |
| Sales orders | Spoken after a meeting in Hindi, Hinglish or English, reviewed, then sent to Accounts. Stock is shown "as of" the last sync, never guessed | ✅ |
| GSTIN checks | Validates format, state code and check digit. A failed check is a warning to confirm, not a block | ✅ |
| Automation rules | "When this happens, do that", with limits, a dry run and a history | ✅ |
| Email | Sent through the company's own mail server, with templates, attachments and an outbox that records honestly whether each email was sent | ✅ |
| Visits | Visit plans, check-in with a single location reading on tap, and a daily activity report | ✅ |
| Calendar feed | A personal calendar feed (tasks, promised payment dates) for Google, Apple or Outlook | ✅ |
| WhatsApp provider | Integration point for the WhatsApp Business API, signed reply webhooks and STOP opt-outs. No vendor is connected yet; the browser extension is the default | 🟡 |

## 7. Dealer–distributor CRM

A **mode** for manufacturers that sell only through distributors. A company that sells
directly never sees it.

| Feature | Description | Status |
| --- | --- | :---: |
| Channel mode & roles | Switching the mode on enables the whole dealer CRM. Adds *Planning* and *Production* (read-only) roles. Every deadline is a setting, and "who can do this" is a rule | ✅ |
| Distributors & dealers | Dealer KYC with the GST certificate, duplicate checks, the state rule, and moves between distributors with full history | ✅ |
| Lead → dealer | *Make a dealer* from any open lead, with its history carried over and the distributor notified | ✅ |
| Price lists | Distributor and dealer price lists, a new version for every change, and a branded dealer price-list PDF that never shows distributor prices | ✅ |
| Dealer quotes | A changed price is approved by the Sales Manager, then by the distributor through a secure, single-use, expiring link. Reminders, escalation and expiry run on a schedule | ✅ |
| Dealer orders | Taken on a visit, a call or from an accepted quote. The distributor is emailed the order PDF | ✅ |
| Discount limits | Each Sales Manager sets their team's discount limit. Anything above it needs approval | ✅ |
| Projections | Each dealer's products × months grid, in pieces and valued at dealer price, with salesperson deadlines and reminders | ✅ |
| Monthly distributor quotation | Built from the dealers' projections at distributor price. The distributor approves, rejects, comments or counter-proposes through a secure link | ✅ |
| Planning lock & factory plan | Planning locks the company total. Changes after the lock need approval. Production gets a read-only factory plan, and projection vs actual scores its accuracy | ✅ |
| Daily visit sheet | Start and end of day, check-in and check-out per visit, photos, next actions, the summary emailed to the manager, and approval for late entries | ✅ |
| *My day* planner | Salespeople plan their own day, with *Plan ahead* and *Coming up*. Visit history for the salesperson and their manager | ✅ |
| Dashboards & reports | *Today*, *Team today*, *Management*, and ten reports (lead funnel, dealers by distributor, quote and order registers, projection vs actual, productivity…) | ✅ |
| Hindi | Every dealer-CRM screen in English and Hindi | 🟡 awaiting native review |

## 8. Meetings & the accountant

| Feature | Description | Status |
| --- | --- | :---: |
| Meeting notes | Write down what a customer said. The product proposes actions, each with the quote it came from, and a person confirms every one | ✅ |
| Pre-meeting brief | What to know before a meeting, with statements made in the meeting checked against the books | ✅ |
| Tally change requests | A printable slip for the accountant, assigned as a task and marked done only when the books show the change | ✅ |
| Meeting transcription | Integration point and per-company usage metering exist. No speech-to-text provider is connected | ⚪ |

## 9. Data onboarding

| Feature | Description | Status |
| --- | --- | :---: |
| Data source per company | Each company chooses demo, Tally or CSV, with a sync interval and a run log in the platform console | ✅ |
| CSV import | Upload, see a report of what *would* happen, then apply. Includes templates, a reason for each rejected row, and safe re-upload | ✅ |
| Tally bridge | When the server can't reach a customer's Tally (the usual case), their machine pushes data to the server using a per-company key | ✅ |
| Tally parser | The XML request builder and reconcilers are built. The parser is waiting on a recorded capture from a real Tally rather than guessed XML | 🟡 |
| Demo data generator | Generates years of realistic trading history for fictional businesses, with seasonality, trends, dead stock and overdue accounts. A validator checks the data is neither too flat nor too extreme | ✅ |

## 10. Platform, security & compliance

| Feature | Description | Status |
| --- | --- | :---: |
| Multi-tenancy | One deployment, many companies. Isolation is enforced automatically in one central place, and a test checks every route | ✅ |
| Permissions | Module access, row-level data scope, view-only access per screen, and an export permission | ✅ |
| Platform console | Create companies and their first administrator, manage seats, view usage analytics and scheduled jobs, see backup age | ✅ |
| Support access | Consented, expiring, read-only. The only way support can see a company's data | ✅ |
| Audit log | Who changed what, for people, permissions and settings. Credentials are never stored in it | ✅ |
| Sign-in security | Attempt limits, revocable and rotating tokens, a password policy, and optional TOTP two-step sign-in that a company can require for administrators | ✅ |
| Privacy (DPDP Act 2023) | Consent per person and channel, export of one person's data, two-step erasure, optional automatic removal of lost leads | ✅ |
| Backups | Nightly dump with daily and weekly retention and a weekly restore drill. Scripts are tested; the backup container is not yet run in production | 🟡 |
| Scheduler | Timed jobs run once per company, with a lease that prevents double runs | ✅ |
| Languages | Per-person language setting, English and Hindi catalogues, and a test that fails if a Hindi string is missing | ✅ |

## 11. Mobile app

A Flutter app for Android and iOS. It **computes nothing**: every figure comes from the API,
and its menu shows only what the server says the person may open.

| Feature | Description | Status |
| --- | --- | :---: |
| Overview & chase | Key figures, plus a collections screen with one-tap call and WhatsApp | ✅ |
| CRM on the phone | Leads (list and board), customers, issues, tasks, promises, inbox, meeting notes, cash | ✅ |
| Dealer CRM on the phone | *Today*, *My day* (start and end of day, check-in and check-out, spoken notes, photos), dealers, order form and projection grid | ✅ |
| English & Hindi | Localised UI | ✅ |
| Release | `flutter analyze` is clean and the test suite passes. Not yet built as an APK or installed on a handset | 🟡 |
| Push notifications | Supported, but off unless built with Firebase configuration | 🟡 |

## 12. WhatsApp Customer 360 extension

A Chrome (Manifest V3) extension, currently a **prototype** 🟡.

- Shows the BizzMind account behind the WhatsApp Web chat you have open: balance, overdue
  amount, credit position, risk and a trend view.
- Can send follow-ups queued in BizzMind when the company enables it.
- Never reads chat contents.

## 13. Landing page

- A Three.js "thread figure" (a head drawn in threads, with the brain highlighted), driven by
  scrolling.
- Reduced-motion and no-WebGL fallbacks, accessibility checks, and frame-time profiling
  across screen widths.
- Login opens in place over the figure.

---

<sub>© 2026 Aryan Raj. All rights reserved. See [COPYRIGHT.md](../COPYRIGHT.md).</sub>

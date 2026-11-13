# BizzMind: architecture

[← Back to README](../README.md)

A high-level view of how BizzMind is put together and why. It shows design choices, not
implementation. No source code is included.

---

## 1. System overview

```mermaid
flowchart LR
    subgraph Users
        OWN["Owner / admin"]
        SM["Sales manager"]
        REP["Field salesperson"]
        ACC["Accounts / planning"]
        CUST["Customer · distributor<br/>(no login)"]
    end

    subgraph Clients
        WEB["Web app<br/>React SPA"]
        MOB["Mobile app<br/>Flutter"]
        EXT["Chrome extension<br/>on WhatsApp Web"]
        PUB["Public pages<br/>statement · quote · approval"]
    end

    OWN & SM & ACC --> WEB
    REP --> MOB
    REP & SM --> EXT
    CUST --> PUB

    WEB & MOB & EXT & PUB --> NGINX["nginx<br/>one origin"]
    NGINX --> API["FastAPI<br/>REST /api/v1"]
    API --> DB[("PostgreSQL")]
    API --> FILES[("File storage<br/>attachments · photos")]
    API -.-> SMTP["Company's own<br/>mail server"]
    API -.-> WA["WhatsApp<br/>(extension or provider)"]
    API -.-> PAY["Payment gateway<br/>(when configured)"]
    API -.-> PUSH["Push notifications<br/>(when configured)"]
    TALLY["Customer's Tally<br/>on an office PC"] -->|"bridge push"| API
```

- **One API for every client.** The web app, the phone, the extension and the public pages
  all use the same versioned REST API. Business logic lives on the server, so the mobile app
  *computes nothing*.
- **One origin.** In production, nginx serves the web app and forwards `/api` to the API
  container, so the browser sees a single origin and needs no CORS setup.
- **Outbound channels stay off until configured.** Without configuration, nothing is sent,
  and the outbox records "not sent" instead of a false "sent".

## 2. Server layers

```mermaid
flowchart TB
    R["API routers<br/>validation · auth · permissions"] --> S["Services<br/>business rules"]
    S --> REPO["Repositories<br/>the only layer that writes SQL"]
    S --> P["Business-data provider<br/>(demo · CSV · Tally)"]
    S --> AN["Analytics<br/>pure DataFrame functions, no I/O"]
    S --> ML["Forecasting & risk<br/>no database access"]
    S --> NAR["Narration<br/>structured results in, wording out"]
    REPO --> DB[("Database")]
    P --> DB
```

| Layer | Responsibility | Never |
| --- | --- | --- |
| API | Request/response contracts, authentication, permission checks | Build queries or hold business rules |
| Services | Business logic | Depend on the web framework |
| Repositories | Data access | Contain business rules |
| Analytics | Pure calculations on tabular data | Perform any I/O |
| Forecasting & risk | Statistical models on analytics output | Query the database |
| Narration | Turn computed results into business English | Query the database |

These boundaries are a large part of why the codebase can carry 600+ endpoints and still
change safely.

## 3. Three seams that carry the design

### 3.1 Where the books come from

```mermaid
flowchart LR
    A["Analytics · forecasting ·<br/>decisions · assistant"] --> I{{"Business-data<br/>interface"}}
    I --> D["Demo data<br/>(generated)"]
    I --> C["CSV import<br/>(dry run, then apply)"]
    I --> T["Tally<br/>(bridge push · in progress)"]
```

All business data reaches the analytics layer through one interface with a fixed column
contract. Moving a company from demo data to CSV or Tally is a setting, not a rewrite. The
same contract tests run against every provider, so a provider that returns the wrong shape
fails loudly.

### 3.2 Narration that can't invent numbers

The narration layer only receives results that were already computed and authorised, and
decides how to word them. It has no database access and no tools of its own. Swapping the
deterministic wording for a live language model could change *the wording*, but never a
figure.

### 3.3 Tenant isolation in one central place

```mermaid
sequenceDiagram
    autonumber
    participant C as Client
    participant A as API
    participant T as Tenant scope
    participant D as Database

    C->>A: Request with access token
    A->>T: Resolve the caller's company and data scope (once per request)
    Note over T: Platform staff are refused<br/>on every business endpoint
    A->>D: Any read
    T-->>D: Company filter applied automatically
    A->>D: Any write
    T-->>D: Company stamped automatically, cross-company writes refused
    Note over T,D: A query with no scope fails.<br/>It never returns everyone's data.
```

Isolation applies to every query, and a convention like "remember to filter by company" fails
the one time someone forgets. So the scope is attached to the database session itself and
enforced automatically on every read and write. A test enumerates every route to back this
up. The few deliberate cross-company reads (sign-in, migrations, platform screens, resolving a
public link) go through one explicitly named, searchable path.

## 4. How *Ask BizMind* answers a question

```mermaid
flowchart LR
    Q["Question in<br/>plain language"] --> R{"Intent<br/>routing"}
    R -->|"no match"| X["Declines, and lists<br/>what it can answer"]
    R -->|"match"| T["One tool from a<br/>fixed allow-list"]
    T --> S["Same analytics service<br/>the screens use"]
    S --> P["Permission and<br/>data-scope checks"]
    P --> N["Narration<br/>with the figures and their source"]
```

- **Free text never becomes a query.** A question selects a capability, not a statement.
- **Refusing is a feature.** A confident answer to a misheard question looks exactly like a
  correct one, so unknown questions are declined.
- **No API key needed.** Routing and wording are deterministic. A live-model provider is an
  interface for later, not a dependency.

## 5. The dealer–distributor monthly cycle

```mermaid
flowchart TB
    L["Lead (a shop)"] -->|"Make a dealer"| DL["Dealer<br/>attached to one distributor"]
    DL --> Q["Dealer quote<br/>from the dealer's price list"]
    Q -->|"price changed?"| SMA["Sales Manager approval"]
    Q --> DA["Distributor approval<br/>secure single-use link"]
    SMA --> DA
    DA --> O["Dealer order"]

    subgraph Month["Each month (dates are settings)"]
        direction TB
        P1["1st: projection period opens"] --> P2["By the 15th: salespeople enter<br/>each dealer's pieces per month"]
        P2 --> P3["16th: monthly distributor quotation<br/>sent at distributor price"]
        P3 --> P4["By the 20th: distributor approves,<br/>rejects or counter-proposes"]
        P4 --> P5["25th: Planning locks the total"]
        P5 --> P6["Factory plan<br/>(Production: read-only)"]
        P6 --> P7["Month over: projection vs actual"]
    end

    DL --> P2
    V["Daily visit sheet<br/>start day · check-in/out · end day"] --> DL
```

Every step is recorded in the document's history. Emails to distributors carry **hashed,
single-use, expiring action links**, so a distributor without a login can approve or reject
safely.

## 6. Background work

A built-in scheduler runs the timed jobs (follow-up rules, reminders, the morning brief,
promise settlement, quote and projection deadlines, the email outbox, Tally sync, activity
roll-up). Each job runs **per company, in that company's time zone, once**: a lease stops two
workers from running the same job. Administrators can see the jobs in the platform console.

## 7. Security principles

- Access tokens with rotating refresh tokens. Logout, password change and "sign out
  everywhere" revoke them.
- Sign-in attempt limits, a password policy, and optional TOTP two-step sign-in.
- Stored integration credentials are encrypted. Public action and ingest keys are stored
  hashed and shown once.
- Public endpoints never contain a company identifier, so writing to another company through
  them is impossible by design, not merely refused.
- Support access requires consent, expires, and is read-only.
- A production configuration guard refuses to start with unsafe defaults.

## 8. Quality & engineering workflow

```mermaid
flowchart LR
    B["Written brief<br/>for the milestone"] --> PL["Plan"]
    PL --> IM["Implement"]
    IM --> G{"Gates"}
    G -->|"fail"| IM
    G -->|"pass"| RV["Review"]
    RV --> DC["Decision log entry<br/>+ progress log"]
    DC --> TAG["Milestone tag"]
```

Every milestone has to pass:

| Gate | What it checks |
| --- | --- |
| **pytest** (2,400+ tests) | API, services, permissions, tenant isolation, an end-to-end business story with a fake mail server and a frozen clock |
| **Vitest** (700+ tests) | Components, hooks, formatting, translation completeness |
| **TypeScript + lint** | No type errors; lint errors kept at zero |
| **Migrations** | A single head, and a full up/down round-trip from an empty database |
| **Browser smoke** (200+ checks) | Drives the real app in a browser: signs in, waits for real data, drills into charts, checks the mobile layout |
| **Flutter** | `flutter analyze` clean, Flutter tests passing |

Every non-obvious choice is written up as a numbered decision (280+ so far), which keeps the
design reviewable after the fact.

---

<sub>© 2026 Aryan Raj. All rights reserved. See [COPYRIGHT.md](../COPYRIGHT.md).</sub>

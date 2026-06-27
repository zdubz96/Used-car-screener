# Project Brief — Marketplace Car Deal Comparator

## One-line summary
A private app that turns messy Facebook Marketplace listings, seller chats, and documents into clean, comparable "deal cards," then helps me judge them against each other — and against the wider market — to find the best car deal.

## Who it's for
Just me (single user). I'm actively shopping several used cars at once and need to compare them without holding everything in my head or doing manual data entry.

## The problem it solves
Every listing arrives in a different, messy form: a wall of listing text, screenshots, scattered seller DMs, and documents (title photos, registration, VIN/history reports, service records). Comparing them apples-to-apples is tedious and error-prone, and "cheapest" isn't the same as "best." This app does the tedious reading and structuring, then layers in judgment.

## Core user flow
1. **Add a car** — paste listing text and any seller messages; optionally upload a document the seller sent (title/registration photo, VIN history PDF, service records).
2. **Auto-extract** — Claude reads all of it and fills structured fields automatically (no manual entry).
3. **Enrich** — the app pulls market comps for that car and adds model-specific context (reliability, expected mileage, known issues).
4. **Review & compare** — each car becomes a deal card; all cars sit in a side-by-side comparison table with a deal score.
5. **Decide** — the app flags risks/inconsistencies, suggests questions to ask the seller, and recommends the best deal with reasoning.
6. **Return later** — everything is saved between sessions, since deals are collected over days/weeks.

## v1 features (confirmed)
- Add / edit / delete cars.
- **Auto-extract from pasted text and uploaded documents** (images + PDFs supported, so a photo of the title or a VIN report can be read directly).
- **Market-value lookup** — web search for comparable cars to estimate fair price and an over/under-priced read.
- **Car-specific nuances** — model reliability reputation, typical/standard mileage for the age, common failure points and ownership-cost notes.
- **Risk & inconsistency flags** — e.g. salvage/rebuilt/lien title, listing claims that contradict the documents, odometer that doesn't match service records, missing VIN, signs of a curbstoner/flipper.
- **Seller questions** — auto-generated based on what's missing or worth verifying for that specific car.
- **Side-by-side comparison table** with a deal score per car.
- **Recommendation** — best deal called out, with plain-English reasoning.

## Deal-scoring model
Weighted by my ranked priorities, with refinements:

| Priority | Factor | How it's scored |
|---|---|---|
| 1 | **Price** | Judged *relative to market value* (from comps), not just absolute lowest — best value wins over merely cheapest. |
| 2 | **Year / age** | Newer scores higher, contextually. |
| 3 | **Mileage** | Compared against expected mileage for the age (~12k mi/yr baseline); below-average mileage scores better. |
| 4 | **Title & condition risk** | Applied as a **penalty + warning overlay**, not a small linear weight — salvage/rebuilt/lien or serious condition issues drag the score down and trigger visible warnings regardless of an attractive price. |
| 5 | **Distance from me** | Minor tiebreaker. |

Model reliability and known-issue data feed the risk/condition layer and the recommendation narrative. Weights are tunable later if the balance feels off.

## Data captured per car
Year, make, model, trim, asking price, estimated market value, mileage, expected mileage for age, location/distance, title status, number of owners, VIN, accident/condition notes, service-history completeness, seller notes, risk flags, open questions, deal score, and my own private notes.

## Technical approach
- Single self-contained app (React or HTML).
- Uses the Claude API for extraction, analysis, market-comp search, and model knowledge.
- **Persistent storage** so cars survive between sessions.
- Accepts pasted text plus **image/PDF uploads** for documents.
- Web search enabled for market comps and reliability context.

## Out of scope for v1 (possible later)
- Photo galleries per car.
- Exporting a summary or shareable report.
- Follow-up reminders to re-contact sellers.
- Negotiation / offer-tracking history.
- Side-by-side cost-of-ownership projections (insurance, fuel, maintenance).

## Open items before/while building
- Confirm build format (React vs. plain HTML) — no functional difference for you; React is a bit cleaner for the comparison table.
- Tune scoring weights once there's real data in it.
- Decide how aggressive the risk flagging should be (warn on anything uncertain vs. only on confirmed issues).

## Next step
Build v1 as a working app, starting with add-car + auto-extract + the comparison table, then layer in comps, risk flags, and the recommendation.

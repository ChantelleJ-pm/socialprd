# CONFLUENCE PAGE DRAFT
# Page title: Non-English Recipe Support — Requirements & Open Questions
# Suggested location: MyRecipes Recipe Clipper > Supporting Docs

---

## Overview

| | |
|---|---|
| **Related PRD** | [Recipe Clipper - PRD](#) *(update link)* |
| **Jira Initiative** | FFT-1: MyRecipe Recipe Clipper |
| **Owner** | Chantelle James |
| **Reviewers** | Luke Pierotti, Aleza Leinwand, Andrea Watson |

The Social Saving PRD (Section 2.6) notes: *"Non-English recipes are supported. Generated content is preserved in the original language."* Phase 1 and Phase 2 have no equivalent requirement, and there is no engineering guidance on display. This page fills that gap.

---

## Language tiers

| Tier | Examples | Additional effort | Competitor bar |
|---|---|---|---|
| Latin-script | French, Spanish, German, Portuguese | Minimal — mostly works today | Table stakes — all major competitors support this |
| Non-Latin scripts | Japanese, Korean, Chinese, Hindi, Thai | Font loading, encoding validation | Paprika supports Japanese/Chinese; others do not |
| Right-to-left (RTL) | Arabic, Hebrew, Farsi | Layout changes — non-trivial retrofit | No competitor currently supports RTL |

---

## Competitor landscape

Research conducted March 2026. Sources: app store listings, public documentation, competitor help centers.

| App | Recipe clipping | Latin-script | Non-Latin | RTL | Notable |
|---|---|---|---|---|---|
| **Paprika** | ✅ URL clipping | ✅ 15 languages | ✅ Japanese, Chinese (since 2012) | ❌ | Most mature language support of any competitor |
| **ReciMe** | ✅ URL + social | ✅ EN, ES, DE, FR, PT | ❌ | ❌ | Publicly states more languages coming |
| **Flavorish** | ✅ URL + social + image | ✅ EN, ES confirmed | ⚠️ Available on Japanese App Store — not confirmed | ❌ | Closest feature-set competitor to MYR Clipper |
| **Mela** | ✅ URL clipping | ✅ EN, FR, DE, IT | ❌ | ❌ | Added **auto-translation** feature Apr 2025 — translates clipped recipes into user's language |
| **Copy Me That** | ✅ URL clipping | ✅ EN only | ❌ | ❌ | Explicitly states scraper is English-optimized only |
| **Deglaze** | ✅ URL + social | ✅ EN, FR confirmed | ❌ | ❌ | iOS-focused, limited public language info |
| **Pluck** | ✅ URL + social + video | Unknown | Unknown | ❌ | AI-first; no public language documentation |
| **Forkee** | ✅ URL + social | Unknown | Unknown | ❌ | Web-based; no public language documentation |

### Key takeaways

- **RTL is a non-issue competitively** — no competitor supports it. Calling it out of scope for MVP is well justified.
- **Non-Latin scripts are a differentiator, not table stakes** — only Paprika has done this properly. If MYR gets there, it stands out.
- **Latin-script is expected** — all competitors support it. Not shipping with Latin-script support (French, Spanish, German recipes) would be a gap.
- **Mela's translation approach is worth watching** — instead of preserving content in the original language, they translate everything into the user's interface language. This is the opposite of what our Social Saving PRD requires. Neither approach is obviously correct; it depends on your user and whether they want to read recipes in their own language or preserve the original.

---

## Open decisions — product to answer

> ⚠️ Engineering is blocked on the items below until product provides direction.

| # | Question | Options |
|---|---|---|
| 1 | Which language tiers are in scope, and for which phase? | (A) Latin-script first, non-Latin as fast-follow / (B) All tiers now |
| 2 | Is RTL layout support in scope for any current phase? | (A) Yes / (B) No — log fast-follow ticket |
| 3 | Can non-English sites be added to the safelist before display is validated? | (A) Yes, if data is clean / (B) No, hold until display meets a bar |
| 4 | Are we localizing the UI (buttons, labels, errors) or only recipe content? | (A) Content only, UI stays English / (B) Full localization (separate initiative) |
| 5 | Is system font fallback for non-Latin scripts acceptable for MVP? | (A) Yes / (B) No — must meet a design bar first |

---

## Engineering requirements by phase

### All phases
- Store and retrieve all recipe data as UTF-8 end-to-end (scraper → Graphene → Resound → FE)
- Moderation service must be tested against non-Latin content before any non-English launch — it was built and evaluated for English only
- Add an optional `language` field to the recipe document schema for future filtering and search use

### Phase 1 — URL clipping
- Confirm whether `inLanguage` from schema.org markup is captured and stored
- Run moderation smoke tests on titles in Japanese, Arabic, Korean, and Spanish with diacritics before adding any non-English site to the safelist

### Phase 2 — OCR + form
- OCR spike must include at least one non-Latin test case (printed and handwritten)
- AI prompting must explicitly instruct the model to respond in the source language — models default to English without this instruction

### Phase 3 — Social saving
- AI prompt must output in the source language (same as Phase 2)
- Store the language detected by the transcription service alongside the recipe

---

## Design asks

Three mocks needed — for each, show current state (no changes) and corrected version:

1. **Recipe card with a non-Latin title** (Japanese or Arabic) — to decide if system font fallback is acceptable for MVP
2. **Recipe card with an RTL title** (Arabic or Hebrew) — to decide if RTL is in scope
3. **Full recipe template** populated with non-English content throughout — to validate layout with long non-Latin text

---

## Out of scope (confirmed)
- UI localization
- Machine translation of recipe content
- Full RTL layout support *(recommend fast-follow ticket)*
- Language-specific search ranking

---

## Actions

| Priority | Action | Owner |
|---|---|---|
| 🔴 | Answer the 5 open decisions above | Chantelle |
| 🔴 | Run moderation smoke tests on non-English titles | Luke / Kevin |
| 🔴 | Confirm UTF-8 storage end-to-end | Luke |
| 🟡 | Create 3 display mocks (see Design asks) | Aleza / Andrea |
| 🟡 | OCR spike to include non-Latin test cases | TBD |
| 🟢 | Add `language` field to recipe document schema | Graphene team |
| 🟢 | Capture `inLanguage` from Phase 1 scraper | Data Services |

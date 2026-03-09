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

The Social Saving PRD (Section 2.6) briefly states non-English recipes are supported, but Phase 1 and Phase 2 have no requirements, and engineering has no guidance on display. This page fills that gap.

---

## Open decisions — product to answer

> ⚠️ Engineering is blocked until these are resolved.

| # | Question | Options |
|---|---|---|
| 1 | Which language tiers are in scope, and when? | (A) Latin-script now, non-Latin as fast-follow / (B) All tiers now |
| 2 | Is RTL layout (Arabic, Hebrew) in scope for any current phase? | (A) Yes / (B) No — log fast-follow ticket |
| 3 | Can non-English sites be added to the safelist before display is validated? | (A) Yes / (B) No — hold until display meets a bar |
| 4 | UI localization or content only? | (A) Recipe content stays in original language, UI stays English / (B) Full UI localization (separate initiative) |
| 5 | Is system font fallback for non-Latin scripts acceptable for MVP? | (A) Yes / (B) No — must meet a design bar |

---

## Language tiers and competitive bar

| Tier | Examples | Competitor bar |
|---|---|---|
| Latin-script | French, Spanish, German | Table stakes — all competitors support this |
| Non-Latin | Japanese, Chinese, Korean | Differentiator — only Paprika does this well |
| RTL | Arabic, Hebrew | No competitor supports this — safe to defer |

**Competitors researched (March 2026):** Paprika, ReciMe, Flavorish, Mela, Copy Me That, Deglaze, Pluck, Forkee.
One callout: Mela shipped auto-translation in April 2025 — they translate clipped recipes into the user's language rather than preserving the original. Opposite of our current PRD direction; worth a conversation with stakeholders.

---

## Engineering requirements

- **All phases:** Store all recipe data as UTF-8 end-to-end. Test moderation service against non-Latin content before any non-English launch — it was built for English only. Add optional `language` field to recipe document schema.
- **Phase 1:** Confirm `inLanguage` from schema.org markup is captured. Run moderation smoke tests (Japanese, Arabic, Korean, Spanish) before adding non-English sites to safelist.
- **Phase 2:** OCR spike must include a non-Latin test case. AI prompt must explicitly instruct model to respond in source language — models default to English without this.
- **Phase 3:** Same AI language instruction as Phase 2. Store language detected by transcription service with the recipe.

---

## Design asks

Three mocks needed — show current state and corrected version for each:

1. Recipe card with a non-Latin title (Japanese or Arabic) — font fallback decision
2. Recipe card with an RTL title (Arabic or Hebrew) — RTL scope decision
3. Full recipe template with non-English content throughout — layout validation

---

## Out of scope
UI localization · Machine translation · RTL layout *(fast-follow ticket recommended)* · Language-specific search

---

## Actions

| Priority | Action | Owner |
|---|---|---|
| 🔴 | Answer the 5 open decisions above | Chantelle |
| 🔴 | Run moderation smoke tests on non-English titles | Luke / Kevin |
| 🔴 | Confirm UTF-8 storage end-to-end | Luke |
| 🟡 | Create 3 display mocks | Aleza / Andrea |
| 🟡 | OCR spike to include non-Latin test cases | TBD |
| 🟢 | Add `language` field to recipe document schema | Graphene team |
| 🟢 | Capture `inLanguage` from Phase 1 scraper | Data Services |

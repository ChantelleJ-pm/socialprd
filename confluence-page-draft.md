# CONFLUENCE PAGE DRAFT
# Page title: Non-English Recipe Support — Requirements & Open Questions
# Suggested location: MyRecipes Recipe Clipper > Supporting Docs
# Label suggestions: recipe-clipper, language-support, engineering, phase-1, phase-2, phase-3

---
PASTE EVERYTHING BELOW THIS LINE INTO CONFLUENCE
---

## Overview

This page captures requirements, open questions, and design decisions related to non-English recipe support across all three phases of the Recipe Clipper. It is intended to help engineering assess impact and help product and design make scoping decisions before work begins.

**Status:** 🟡 In Review — decisions needed from product and design (see Open Decisions table below)

| | |
|---|---|
| **Related PRD** | [Recipe Clipper - PRD](#) *(update with your PRD link)* |
| **Jira Initiative** | [FFT-1: MyRecipe Recipe Clipper](https://dotdash.atlassian.net/browse/FFT-1) |
| **Owner** | Chantelle James |
| **Last Updated** | March 2026 |
| **Reviewers** | Luke Pierotti, Aleza Leinwand, Andrea Watson |

---

## Why this matters

The Social Saving PRD (Section 2.6) includes one line: *"Non-English recipes are supported. Generated content is preserved in the original language."* However, there are no requirements for Phase 1 (URL clipping) or Phase 2 (OCR + form), and no engineering guidance on how foreign-language content should be stored or displayed on MyRecipes.

This page fills that gap.

---

## Language categories and effort level

Non-English support covers three tiers with meaningfully different technical complexity.

| Category | Example languages | Display concern | Effort |
|---|---|---|---|
| Latin-script | French, Spanish, German, Italian, Portuguese | Diacritics (é, ñ, ü) — mostly works today | Low |
| Non-Latin scripts | Japanese, Korean, Chinese, Arabic, Hebrew, Thai, Hindi | Font loading, character encoding | Medium |
| Right-to-left (RTL) | Arabic, Hebrew, Farsi | Layout mirroring required | High |

---

## Open Decisions — Product to answer

> ⚠️ Engineering is blocked on the items below until product provides direction. These are not engineering decisions.

| # | Question | Options | Owner | Due |
|---|---|---|---|---|
| 1 | Which language tiers are in scope, and for which phase? | (A) Latin-script only for now, non-Latin as fast-follow / (B) All three tiers at once | Chantelle | TBD |
| 2 | Is RTL layout support (Arabic, Hebrew) in scope for any current phase? | (A) In scope — engineer now / (B) Out of scope — log fast-follow ticket | Chantelle + Luke | TBD |
| 3 | Can non-English sites be added to the safelist before display support is confirmed? | (A) Yes, if data is clean / (B) No, hold until display meets a quality bar | Chantelle | TBD |
| 4 | Are we localizing the MyRecipes UI (buttons, labels, errors) or only the recipe content? | (A) Content only — UI stays English / (B) Full UI localization (separate initiative) | Chantelle | TBD |
| 5 | Is system font fallback for non-Latin scripts acceptable for MVP? | (A) Yes, good enough for now / (B) No, must meet a design bar first | Chantelle + Aleza / Andrea | TBD |
| 6 | What is the OCR accuracy threshold for non-Latin handwritten recipes? (Phase 2) | (A) Same 85%+ bar as English / (B) Best-effort, explicitly out of scope for Phase 2 launch | Chantelle | TBD |

---

## Phase 1 — URL Clipping

### What engineering needs to validate

| # | Question | Who answers |
|---|---|---|
| 1 | Does the scraper normalize all responses to UTF-8 before storing to Graphene? | Data Services (Todd / Vijay) |
| 2 | Is the `inLanguage` field from schema.org Recipe markup captured and stored? | Data Services |
| 3 | Does the moderation service incorrectly flag or reject non-Latin titles (Japanese, Arabic, Korean, Cyrillic)? | Engineering (Luke / Kevin) |
| 4 | Do MyRecipes web fonts cover non-Latin characters, or does the browser fall back to a system font? | FE (Luke / Jesse) |

### Requirements

- Scraper must store recipe content as UTF-8; behavior when source encoding differs must be documented
- Before any non-English domain is added to the safelist, run moderation smoke tests against titles in at least: Japanese, Arabic, Korean, and Spanish with diacritics
- Define a policy for whether non-English sites can be safelisted before display-layer support is confirmed *(see Open Decisions #3)*

---

## Phase 2 — Manual Form & OCR

### What engineering needs to validate

| # | Question | Who answers |
|---|---|---|
| 1 | What is OCR accuracy for non-Latin handwritten recipes (e.g., Japanese recipe card)? | Engineering spike |
| 2 | Does the AI prompting strategy output content in the source language, or does it default to English? | Engineering (OpenAI spike) |
| 3 | Does the moderation service produce false positives on common non-English cooking terms? | Engineering (Luke / Kevin) |

### Notes on the manual form

Modern browsers handle non-Latin input in text fields natively — users can type in any language. The form itself will work. The risks are moderation and rendering, not input.

### Requirements

- OCR spike must include at least one non-Latin script test case (Japanese or Chinese, printed and handwritten)
- AI prompting must explicitly instruct the model to output in the same language as the source content — do not translate
- Moderation service must not reject content based solely on unrecognized character sets
- RTL layout decision needed before template work begins *(see Open Decisions #2)*

---

## Phase 3 — Social Saving (App)

The Social Saving PRD already states: *"Non-English recipes are supported. Generated content is preserved in the original language."* The following detail is needed for engineering to implement this correctly.

| Requirement | Detail |
|---|---|
| AI output language | Prompt must explicitly instruct the model to respond in the source language. Without this, models default to English. |
| Language detection | The transcription service (e.g., Whisper) returns a detected language. Confirm this is stored with the recipe for future use. |
| Number and unit formatting | European recipes may use decimal commas (1,5 kg). If ingredients are structured data, the model must accommodate this. If stored as free text, this is handled automatically. |
| Font and RTL rendering | Same concerns as Phase 1 and Phase 2 apply. |

---

## Cross-cutting engineering requirements

### Character encoding (all phases)

All recipe data must be stored and retrieved as UTF-8 end-to-end.

- **Graphene:** Confirm field storage is UTF-8 with no byte-length limits that would truncate multibyte characters. (A 50-character Japanese title = ~150 bytes in UTF-8.)
- **Resound APIs:** Must pass through non-ASCII content without escaping or truncating.
- **Frontend:** Must not re-encode or escape UTF-8 characters at render time.

### Font strategy

MyRecipes loads a limited web font set designed for English. Options for non-Latin characters:

| Approach | Effort | Quality | Recommendation |
|---|---|---|---|
| System font fallback | Low | Inconsistent but functional | ✅ Acceptable for MVP |
| Unicode-range font subsetting (e.g., Google Noto) | Medium | Consistent non-Latin rendering | Fast-follow after MVP |
| Full CJK / Arabic font loading | High | Best quality | Not recommended — bandwidth cost |

### RTL layout

Arabic and Hebrew require `dir="rtl"` on containers and CSS logical properties throughout recipe card and template components. This is a non-trivial retrofit.

> **Recommendation:** Explicitly call RTL out of scope for the current phase. Log a dedicated fast-follow ticket so it does not get lost. *(See Open Decisions #2)*

### Language field in data schema

We currently have no way to know what language a saved recipe is in. Recommend adding an optional `language` field to the recipe document. Sources:

- Phase 1: `inLanguage` from schema.org markup
- Phase 2: Language detected from extracted text
- Phase 3: Language returned by transcription service

This enables future filtering, search indexing, and personalization.

---

## Design asks

Design input is needed on the following before engineering finalizes scope.

| Mock needed | Purpose |
|---|---|
| Recipe card with non-Latin title (Japanese or Arabic), system font fallback vs. corrected | Decide if font fallback is acceptable for MVP |
| Recipe card / quick view with RTL title (Arabic or Hebrew) — current state and corrected | Decide if RTL is in scope |
| Full recipe template (title, ingredients, directions) with non-English content throughout | Validate layout holds for long non-Latin text |

Each mock should show: (1) what it looks like today with no changes, and (2) what the corrected version should look like.

---

## Out of scope (confirmed)

The following are explicitly not part of the current initiative. They should be tracked as future phases.

- UI localization — translating MyRecipes interface labels, buttons, and error messages
- Machine translation of recipe content
- Language-specific search ranking or filtering
- Full RTL layout support (deferred — fast-follow ticket recommended)

---

## Immediate actions

| Priority | Action | Owner | Status |
|---|---|---|---|
| 🔴 High | Product to answer all 6 Open Decisions | Chantelle | Not started |
| 🔴 High | Run moderation smoke tests on non-English recipe titles | Luke / Kevin | Not started |
| 🔴 High | Confirm UTF-8 storage end-to-end: scraper → Graphene → Resound → FE | Luke | Not started |
| 🟡 Medium | Design to create mocks for 3 non-English display scenarios | Aleza / Andrea | Not started |
| 🟡 Medium | OCR spike to include non-Latin test cases | TBD | Not started |
| 🟡 Medium | Update AI prompting spike to test non-English transcripts | TBD | Not started |
| 🟢 Low | Add `inLanguage` capture to Phase 1 scraper | Data Services | Not started |
| 🟢 Low | Add `language` field to recipe document schema | Graphene team | Not started |

# Non-English Recipe Support — Requirements & Engineering Guidance

**Branch:** `cursor/recipe-language-support-749b`
**Related PRD:** Recipe Clipper - PRD.pdf
**Status:** Draft — for engineering review

---

## Background

The existing PRD covers three saving flows:
- **Phase 1 (MVP):** URL clipping from safelisted sites
- **Phase 2:** Manual recipe form + OCR (image-to-text)
- **Phase 3 / App:** Social saving from TikTok and Instagram

The Social Saving section (Section 2.6) includes one line: *"Non-English recipes are supported. Generated content is preserved in the original language."* However, neither Phase 1 nor Phase 2 address non-English handling, and there is no engineering guidance on how foreign-language content should behave at the display layer for any phase.

This document captures open questions, requirements, and known considerations so that engineering can assess impact and flag blockers before work begins.

---

## Scope of Languages

Non-English support encompasses three broad categories with meaningfully different technical implications:

| Category | Examples | Key concerns |
|---|---|---|
| Latin-script languages | French, Spanish, Portuguese, Italian, German | Diacritics, special characters (é, ñ, ü) — low display risk |
| Non-Latin scripts | Japanese, Korean, Chinese (Simplified/Traditional), Arabic, Hebrew, Thai, Hindi | Font loading, character encoding, RTL layout |
| Right-to-left (RTL) | Arabic, Hebrew, Farsi | Bidirectional text, layout mirroring |

---

## Phase 1 — URL Clipping

### Current state
The scraper extracts title, image, and source URL from third-party recipe sites using JSON-LD / schema.org `Recipe` markup. No language filtering is applied.

### Open questions for engineering

1. **Character encoding** — Does the scraper normalize responses to UTF-8 before storing to Graphene? If a foreign site returns content in ISO-8859-1, Shift-JIS, or another encoding, what happens to the scraped title?

2. **Schema markup language** — The `schema.org/Recipe` spec supports an `inLanguage` property. Are we capturing or storing this? If not, we have no way to know what language a saved recipe is in.

3. **Moderation service compatibility** — The text moderation service is currently evaluated for English content. Does it flag or reject titles containing CJK characters, Arabic, Cyrillic, etc. as false positives? This needs a direct test before any non-English sites are added to the safelist.

4. **Safelist scope** — Today's safelist is English-language sites. If we want to add non-English sites (e.g., German cooking blogs, Japanese recipe sites), the safelist review and approval process needs to account for the domain's language.

5. **Title display on recipe cards** — Recipe card titles are currently rendered with a single web font stack. Do the fonts loaded for MyRecipes cover the characters needed for Japanese, Korean, Chinese, Arabic, or Hebrew? If not, the browser will fall back to a system font that may look inconsistent.

6. **RTL text in cards** — A recipe card title in Arabic or Hebrew is right-to-left. The current card layout is LTR-only. Displaying RTL text in an LTR container will not visually break, but the text alignment will look wrong. Are we expected to handle this for MVP, or is it a fast-follow?

### Requirements to add to Phase 1

- [ ] Scraper must store content in UTF-8; document behavior when source encoding differs.
- [ ] Determine whether `inLanguage` from schema markup is captured and stored in Graphene.
- [ ] Run moderation service smoke tests against titles in Japanese, Arabic, Korean, and Cyrillic before any non-English domain is added to the safelist.
- [ ] Define a policy for whether non-English sites can be safelisted before display-layer language support is confirmed.

---

## Phase 2 — Manual Form & OCR

### Manual recipe form
Users may type recipe content in any language. The form accepts free text, so foreign scripts will work in input fields as long as the browser supports them (which modern browsers do). The main risk is moderation and storage.

### Open questions for engineering

1. **OCR language support** — Which OCR or AI service is being used for image-to-text extraction? Most general-purpose OCR tools have strong support for Latin scripts and reasonable support for CJK, but accuracy varies significantly for handwritten content in non-Latin scripts. If a user photographs a handwritten Japanese recipe card, what is the expected accuracy?

2. **AI prompt language** — If OpenAI (or another model) is used to structure the extracted text, the prompt strategy needs to be language-aware. A prompt written in English that says "extract the recipe title, ingredients, and steps" may not produce consistent output when the source content is in another language. The spike for AI prompting strategy (Spike 4 in the Social Saving section) should address this.

3. **Moderation of non-English text** — The text moderation service needs to handle non-English content without producing excessive false positives. For example, common cooking terms in Arabic or Chinese should not be flagged as offensive.

4. **Form field labels and UI copy** — The form itself (labels, placeholder text, error messages, CTAs) is currently English-only. If we expect users to submit non-English recipes, should the form UI also be localized? This is a separate and larger scope question — confirm with product whether UI localization is in or out of scope.

5. **Recipe template rendering** — The personal recipe template (Section 2.2 of the OCR PRD) will display whatever text the user saved. If that text is in Arabic, the ingredient list and directions will render LTR in an LTR layout, which looks wrong. Same concern as Phase 1 for RTL languages.

### Requirements to add to Phase 2

- [ ] Spike to confirm OCR accuracy for at least one non-Latin script (e.g., Japanese or Chinese) against handwritten and printed recipe cards.
- [ ] Confirm AI prompting strategy handles non-English source content — output language should match input language (do not translate).
- [ ] Moderation service must not reject non-English content based solely on unrecognized characters.
- [ ] Decide whether RTL layout support is in scope for Phase 2 or a future phase.

---

## Phase 3 — Social Saving (App)

### Existing requirement (Section 2.6)
> "Non-English recipes are supported. Generated content is preserved in the original language."

This is the right intent. The following are additional details engineering will need.

### Open questions for engineering

1. **AI output language** — The AI prompt must explicitly be instructed to output content in the same language as the source transcript. Without this, many models will default to English output even when given non-English input.

2. **Transcript language detection** — Audio transcription services (e.g., Whisper) do detect language automatically. Confirm that the chosen transcription service returns the detected language, and decide whether to store it alongside the recipe for future use (e.g., filtering or grouping in UI).

3. **Ingredient quantities and units** — Some languages use different number formatting (e.g., European decimal commas) or different measurement systems. If the AI structures ingredients as structured data (amount + unit + name), does the data model accommodate these variations, or is it stored as free text?

4. **Rendering** — Same concerns as Phase 1 and Phase 2 for font coverage and RTL layout apply here.

---

## Cross-Cutting Engineering Considerations

### 1. Character encoding
All recipe data must be stored and retrieved as UTF-8. Confirm that:
- Graphene stores recipe fields as UTF-8 strings with no character-length limits that would truncate multibyte characters (a 50-character CJK title uses 150 bytes in UTF-8).
- Resound APIs pass through non-ASCII content without escaping or truncating.
- The frontend does not encode/escape UTF-8 characters in a way that breaks display.

### 2. Font coverage
MyRecipes currently loads a limited set of web fonts designed for English-language content. To reliably display non-Latin scripts, one of the following approaches is needed:
- **System font fallback** (lowest effort): Accept that non-Latin text will render in whatever system font the user's device provides. Visually inconsistent but functional.
- **Unicode-range font subsetting** (medium effort): Load additional font subsets (e.g., Google Fonts Noto) only when non-Latin characters are detected.
- **Full CJK / Arabic font loading** (high effort, high bandwidth): Not recommended for MVP.

Engineering recommendation: confirm with design whether system font fallback is acceptable for an MVP of non-English support.

### 3. RTL layout
Arabic and Hebrew are written right-to-left. Displaying them correctly requires either:
- Setting `dir="rtl"` on the container element when RTL content is detected.
- Using CSS logical properties (`margin-inline-start` instead of `margin-left`) throughout the recipe card and template components.

This is non-trivial to retrofit onto existing components. **Recommendation: explicitly scope RTL support out of the MVP and log it as a fast-follow with a clear ticket.**

### 4. Language detection and storage
We currently have no mechanism to know what language a saved recipe is in. For future features (filtering by language, surfacing recipes in a user's preferred language, search indexing), it would be useful to store a `language` field on each recipe document. Options:
- Capture `inLanguage` from schema.org markup (Phase 1).
- Detect language from the extracted text using a lightweight library (e.g., `franc`, `langdetect`) before saving.
- Accept the detected language from the AI or transcription service (Phase 3).

### 5. Search indexing
External recipes do not appear in MyRecipes search (by current design). Personal recipes are private. So non-English content does not immediately affect public search. However, if future phases surface saved recipes in search, the search index must support non-English tokenization — this is a separate dependency on the Search & Discovery team.

### 6. Moderation
The text moderation service should be tested and documented for:
- Non-Latin scripts (does it error, pass, or incorrectly flag?)
- Common false positives for specific languages (e.g., German words that contain substrings flagged by English-language profanity filters)
- Whether the service has configurable language modes

---

## Recommended Immediate Actions

| Priority | Action | Owner |
|---|---|---|
| High | Run moderation service tests against non-English recipe titles (Japanese, Arabic, Korean, Spanish with diacritics) | Engineering (Luke / Kevin) |
| High | Confirm UTF-8 storage end-to-end: scraper → Graphene → Resound → FE | Engineering |
| High | Define scope: is RTL layout support in or out of MVP for non-English? | Product (Chantelle) + Design (Aleza / Andrea) |
| Medium | Confirm font fallback behavior for non-Latin scripts on MyRecipes recipe cards | Engineering + Design |
| Medium | Add `language` field to recipe document schema as an optional stored field | Engineering (Graphene team) |
| Medium | Update AI prompting spike (Spike 4, Social Saving) to include non-English transcript test cases | Engineering |
| Low | Add `inLanguage` capture to Phase 1 scraper | Engineering (Data Services) |

---

## Out of Scope (for now)

- UI localization (translating MyRecipes interface labels, buttons, and error messages into other languages)
- Machine translation of recipe content
- Language-specific search ranking or filtering
- Full RTL layout support in recipe cards and templates

These should be tracked as future-phase considerations once the core non-English data pipeline is validated.

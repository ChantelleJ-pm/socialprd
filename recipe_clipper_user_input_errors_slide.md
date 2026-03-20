# Recipe Clipper user-input errors stakeholder slide

## Title
Most user-input errors happen because users are not pasting a recipe URL at all

## Subtitle
Post-2/19 failed-URLs analysis of `user_input_error` entries only (`n = 988`)

## Core breakdown

- **44.2%** blank / empty submit
- **25.8%** search terms or recipe names instead of a URL
- **11.1%** email address entered
- **10.7%** malformed or partial URL pasted
- **8.1%** site/domain only, not a recipe-page URL

## Stakeholder takeaway

- This is **not mainly an email problem**
- The biggest issue is that many users are **not attempting to paste a valid recipe-page URL**
- That suggests a **UX guidance and validation problem** as much as a parsing problem

## What this points to

- disable save on empty submit
- update the field guidance to say **Paste a recipe page URL**
- show tailored inline validation for:
  - empty submit
  - recipe names / search terms
  - email addresses
  - homepage/domain-only inputs
  - malformed URLs

## Example inputs by bucket

- **Blank:** `""`
- **Search terms / recipe names:** `chicken`, `goulash`, `strawberry dump cake`
- **Emails:** `ccabylis@gmail.com`
- **Malformed / partial URLs:** `https//liveloveandsugar.com`, `southern living.com/best carrot cake`
- **Site/domain only:** `eatingwell.com`, `allrecipes.com`, `MyRecipes`

## Footnote
Counts are from the detailed Failed URLs tab, filtered to `user_input_error` rows in the trusted post-2/19 period.

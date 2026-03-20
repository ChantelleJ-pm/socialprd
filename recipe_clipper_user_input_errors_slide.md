# Recipe Clipper user-input errors stakeholder slide

## Title
User-input errors are mostly caused by missing or non-URL input

## Subtitle
All-time raw Failed URLs analysis of `user_input_error` entries (`n = 4,326`)

## Visual

- Use a **horizontal bar chart** sorted largest to smallest

## Core breakdown

- **48.4%** blank / empty submit (**2,094**)
- **26.3%** search terms or recipe names instead of a URL (**1,137**)
- **10.6%** email address entered (**458**)
- **7.9%** site/domain only, not a recipe-page URL (**340**)
- **6.9%** malformed or partial URL pasted (**297**)

## Key callouts

- **This is not mainly an email problem or a broken-link problem**
- **Only 6.9%** of user-input errors look like malformed URLs
- Among **nonblank entries only**, **50.9%** are search terms or recipe names instead of a URL

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
- **Site/domain only:** `eatingwell.com`, `allrecipes.com`, `MyRecipes`
- **Malformed / partial URLs:** `https//liveloveandsugar.com`, `southern living.com/best carrot cake`

## Footnote
Counts are from the detailed Failed URLs tab, filtered to all-time `user_input_error` rows.

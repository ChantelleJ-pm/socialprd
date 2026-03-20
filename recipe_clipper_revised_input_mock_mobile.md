# Recipe Clipper revised mobile input mock

## Goal
Translate the revised Add Recipe experience into an app-native mobile flow.

## Screens included

### Screen 1 - Default state
- App header: **Add recipe**
- Short description: **Paste a recipe page URL to save it to MyRecipes.**
- Field label: **Recipe page URL**
- Placeholder: **https://www.allrecipes.com/recipe/...**
- Helper text: **Use a recipe page link — not a recipe name or site homepage.**
- Secondary actions:
  - **Paste from clipboard**
  - **Supported sites**
- Primary action:
  - **Save recipe** stays disabled until input looks valid

### Screen 2 - Validation state
- Entered value: **chicken soup**
- Inline error:
  - **This looks like a recipe name. Paste the recipe page URL instead.**
- Recovery hint:
  - **Example: https://www.allrecipes.com/recipe/...**
- Keep input visible so the user can correct it without restarting

## Design intent

- Make the flow clearly feel like **paste a recipe link**, not search
- Put the most useful actions near the field:
  - paste from clipboard
  - supported sites
- Use short, reason-specific inline validation
- Keep the primary CTA visible but disabled until input is actionable

## Validation copy set

- **Empty input**
  - **Paste a recipe page link to continue.**
- **Recipe name / search term**
  - **This looks like a recipe name. Paste the recipe page URL instead.**
- **Email address**
  - **This looks like an email address. Paste a recipe link instead.**
- **Site/domain only**
  - **Paste the full recipe page URL, not just the site homepage.**
- **Malformed URL**
  - **We couldn't read that link. Try pasting the full recipe page URL again.**

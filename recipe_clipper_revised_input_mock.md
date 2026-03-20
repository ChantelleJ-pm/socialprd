# Recipe Clipper revised input mock

## Goal
Reduce the biggest user-input failure modes seen in the Failed URLs data:

- blank / empty submit
- search terms or recipe names instead of a URL
- email addresses
- site/domain-only input
- malformed / partial URLs

## Recommended experience

### Default state
- Modal title: **Add recipe**
- Description: **Paste a recipe page URL to save it to MyRecipes.**
- Field label: **Recipe page URL**
- Placeholder: **https://www.allrecipes.com/recipe/...**
- Helper text: **Use a recipe page link — not a recipe name or site homepage.**
- Secondary actions:
  - **Paste from clipboard**
  - **Supported sites**
- Primary action:
  - **Save recipe** stays disabled until input looks valid
- Assistive note:
  - **If you paste extra text with a link, we'll try to extract the URL automatically.**

### Validation state example
- Entered value: **chicken soup**
- Inline validation:
  - **This looks like a recipe name. Paste the recipe page URL instead.**
- Recovery hint:
  - **Example: https://www.allrecipes.com/recipe/...**

## Tailored validation copy

- **Empty input**
  - **Paste a recipe page link to continue.**
- **Search term / recipe name**
  - **This looks like a recipe name. Paste the recipe page URL instead.**
- **Email address**
  - **This looks like an email address. Paste a recipe link instead.**
- **Site/domain only**
  - **Paste the full recipe page URL, not just the site homepage.**
- **Malformed URL**
  - **We couldn't read that link. Try pasting the full recipe page URL again.**

## Why this mock addresses the data

- Makes the field clearly a **paste-a-link flow**, not search
- Prevents empty submits before they become save attempts
- Gives users reason-specific feedback instead of a generic URL failure
- Encourages recovery without forcing the user to restart
- Creates room for light auto-correction of common pasted input issues

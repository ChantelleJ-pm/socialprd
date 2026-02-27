# Recipe Clipper (MyRecipes) — Product Requirements (Cleaned)

## Summary
Recipe Clipper lets MyRecipes users save recipes from elsewhere on the internet by pasting a URL. The system extracts safe, minimal metadata, stores a cached image via the UGC image service, and saves the recipe as a bookmark that behaves like existing MyRecipes saves (Favorites + Collections) with an outbound “View recipe” link to the original source.

## References (source artifacts)
- Initiative ticket: `HOME-3589` / `FFT-1` (MyRecipe Recipe Clipper)
- Technical approach + agreements: “Recipe Clipper: Technical Approach and Agreements”
- Safelist + schema requirements: “Recipe clipper data” / “Recipe safelist doc”
- Event tracking + reporting: “Recipe_Clipper_Event_Tracking_2025” / “Recipe Clipper Reporting”
- UX/design: Figma (“myr-recipe-clipper”), IA (“MyRecipes Recipe Clipping IA”)

## Problem / Opportunity
Users currently save recipes across browser bookmarks, screenshots, notes, and other apps. MyRecipes is strong for network-owned content, but is not yet a universal recipe box. Adding external saving should increase saves per user and bring users back to manage and cook from their saved recipes.

## Goals
- **User goal**: one consistent place to save and retrieve recipes from anywhere.
- **Product goal**: increase saves per user and collection creation; grow repeat sessions.
- **Business goal**: improve engagement/retention; support acquisition via “save from anywhere” positioning.

## Non-goals (Phase 1 / MVP)
- Hosting or rendering full third‑party recipes on MyRecipes (we link back to the source).
- Bypassing paywalls or bot protections.
- Making external recipes searchable in global MyRecipes search (or public search).
- Ongoing background maintenance/refresh of clipped data (title/image changes, etc.).
- Automatic handling of dead links (404/redirect) beyond letting the link fail.

## Target users
- Existing MyRecipes users who want to save recipes from outside the Dotdash Meredith network.

## Success metrics (initial)
- **Engagement**: saves per user; number of users who save ≥1 external recipe; collection creation rate.
- **Retention**: repeat sessions tied to saving/organizing/cooking from saved recipes.
- **Acquisition (secondary)**: new signups attributable to “save from anywhere” messaging.

## Risks and mitigations
- **SEO authority leakage**: outbound links boost other publishers  
  - Mitigation: apply `rel="nofollow"` to all external links.
- **Legal/copyright**: displaying third‑party images or “rich” fields could be risky  
  - Mitigation: legal sign-off; store images via UGC image service; keep to a minimal, safe field set; remove risky fields if needed.
- **Session depth**: users leave MyRecipes via outbound links and don’t return  
  - Mitigation: open outbound links in a new tab; emphasize organizing flows and surface relevant MyRecipes content where appropriate.
- **Brand dilution**: external recipes overshadow owned content  
  - Mitigation: prioritize/badge MyRecipes recipes in saved views.
- **Scraping reliability**: missing/blocked metadata breaks saves  
  - Mitigation: safelist domains + schema requirements; fallbacks; monitor failures; user-facing “can’t save this site” messaging.
- **Security/moderation**: malicious or offensive content in scraped title/image  
  - Mitigation: block explicit images/offensive language via moderation safeguards; restrict to safelisted domains.

## Assumptions (Phase 1)
- Legal approval is required for what fields we display/store (especially images).
- MVP supports URL copy/paste only (no share sheet, browser extension, or social clipping).
- Clipped recipes are not hosted on MyRecipes; users are always linked back to the original source.
- No ongoing maintenance: once clipped, the saved card is the user’s snapshot at time of save.

## Scope
### Phase 1 / MVP (URL paste)
#### Entry points
- Homepage: “Add Recipe” / “Save a Recipe from Anywhere” module (desktop + mobile)
- Favorites page: “Add Recipe” entry point

#### Core user flow (happy path)
1. User clicks **Add Recipe**.
2. User pastes a recipe URL and presses **Save**.
3. System attempts to import recipe metadata from the URL (schema/structured data).
4. Save modal opens (same pattern as brand sites):
   - Shows recipe image + title
   - Shows list of Collections
   - User selects a Collection and/or presses **Done**
5. The saved recipe behaves like other bookmarks:
   - Appears in Favorites and selected Collections
   - Renders as a card with attribution to the source domain
   - Clicking opens a Quick View modal with key metadata and **View Recipe** outbound link
   - User can unsave (deletes the user’s copy)

#### Data captured (Phase 1 baseline)
**Must capture**
- Source URL (canonicalized as needed)
- Title (when available)
- Image (cached/stored via UGC image service; served from CDN)
- Source attribution (domain / publisher label)

**May capture if validated as safe**
- Deck/description
- Star rating + rating count
- Total time

#### Validation + error handling
- **Duplicate save prevention**: prevent saving the same recipe more than once per user.
- **Non-recipe URL**: show error (“We couldn’t find a recipe at that link.”); do not save.
- **Unsupported domain / missing schema**: show error (“Looks like we can’t save this one because the site doesn’t allow it.”); do not save.
- **Paywall/login blocked**: do not bypass; show the same “site doesn’t allow it” message.
- **Missing fields**: save what we can; allow later edit if the product supports it (not required for MVP).
- **Slow scrape**: show clear loading/progress messaging (target competitor-competitive experience; don’t “hang” silently).

#### Performance expectations (initial)
- Competitive range observed: ~2–5s typical; up to ~8s on slower apps.
- Current scrape response time range discussed: ~2–9s depending on cold starts and site speed.
- Phase 1 decision captured in source notes: proceed with “continue as-is” (Option A), with UX loading messaging to set expectations.

### Phase 2 (future)
- Manual recipe creation via an input form.
- OCR-based recipe creation from an uploaded photo/scan of handwritten recipes.
- (Potential) link health checks and “Original recipe no longer available” messaging.

### Phase 3 (future considerations)
- Social sources (Instagram/TikTok).
- Possible inclusion of external recipes in search (depends on search team constraints).
- Automatic refresh/updates of saved card data.

## Legal + compliance requirements (Phase 1)
### Criteria for adding a site (safelist)
- Publicly available recipes; no login/paywall required.
- Recognized recipe publisher; not an untrusted UGC forum/unknown aggregator.
- Domain reputation verified.
- Technically compatible structured recipe pages (not PDFs/screenshots).

### Approval + monitoring
- Product proposes domains; legal reviews ToS/copyright risk; final sign-off recorded.
- Maintain a central registry (spreadsheet/Airtable/internal tool).
- Periodic review (quarterly/semi-annual) for paywalls, ToS changes, ownership changes.
- Track takedown/complaints; repeated issues trigger removal.
- **Removal/opt-out**: immediate removal if a site requests opt-out or via takedown request.

### Display rules
- Clear attribution: recipe title + domain + outbound link.

## Technical requirements + safeguards (Phase 1)
### Scraping approach
- Restrict clipping to **safelisted domains** that meet legal and technical criteria.
- Use structured schema/Recipe metadata where available.
- Extract key fields: title, image, URL (and other safe fields if approved).
- If schema is missing: do not “scrape around” it for MVP; show an error message (and do not save).

### Safeguards
- Block explicit images/offensive language via moderation safeguards.
- Prevent duplicate saves (per user).

### Image storage and indexing safeguards
- Store third-party images via the UGC image service (not directly in the DB).
- Cache images on CDN; FE must render from CDN (not directly from third-party hosts).
- Images must not be reused in CMS or across brands.
- If image URLs are publicly accessible, responses must include `x-robots-tag: none` to prevent search indexing.

### SEO
- Apply `rel="nofollow"` to all external links.

## Dependencies / teams (as captured)
- Frontend (Flex 2)
- Backend services (Graphene, Resound, Mantle, Data Services)
- Legal
- RevDev (ad placement considerations)
- QA/UAT

## Rollout (initial captured plan)
- Target timeframe referenced: Q4 2025
- Example milestone sequence captured in source: Dev complete → QA/UAT kickoff → target launch

## Open questions (to resolve before final MVP sign-off)
- **Field set**: which “rich” metadata fields (deck/ratings/time) are approved by legal to store and display?
- **Domain gating**: will the UX enforce a safelist-only domain picker vs. accepting any URL and failing gracefully?
- **Schema fallback**: one source artifact mentions a fallback (“if no structured data, save URL + image only”). Is that in-scope for Phase 1, or explicitly deferred?
- **Dead links**: do we want any MVP handling for 404/redirect, or leave entirely to the user until Phase 2?
- **Moderation timing**: can moderation run inline without creating unacceptable save latency? If not, what’s the fallback UX?
- **Attribution label**: can we display the source name/domain consistently; any legal wording requirements?


# Recipe Clipper monthly stakeholder review

_Copy/paste-ready draft for Confluence_

## Page purpose

This page is meant to be the standing stakeholder readout for Recipe Clipper. It should make it easy to review:

- how the product is performing
- where friction remains
- what the team is doing next
- what changed month over month

---

## At a glance

**Last updated:** 3/19/2026  
**Audience:** GM, Product, Engineering, Design, Editorial, Analytics

### Current headline

Recipe Clipper is delivering meaningful launch-to-date value, and the core save experience has improved since launch. The biggest remaining opportunities are expanding site coverage and reducing non-URL / empty-submit input errors.

---

## Current KPI summary

### Launch-to-date (through 3/19/2026)

| KPI | Current value | Why it matters |
|---|---:|---|
| Add Recipe clicks | **23,803** | Demand / discovery of the feature |
| Save Recipe clicks | **16,906** | Save attempts |
| Successful recipes saved | **8,624** | Delivered product value |
| Save success rate | **51.0%** | Core flow health |
| Avg save-click time | **4.65s** | Experience speed / perceived quality |
| User error rate | **25.9%** | Input friction |
| Product error rate | **7.0%** | Internal reliability issues |
| External/domain error rate | **3.3%** | Domain / coverage constraints |
| External-brand share of successful saves | **81.7%** | Confirms intended external clipping use case |
| Supported Sites clicks | **1,931** | Demand for domain compatibility information |

### Recommended north-star

**Successful saves per active user**

This is still the recommended north-star KPI for the product as a whole, but it is **not available in the current export** because we do not yet have a deduped launch-to-date active-user denominator for the full launch window.

---

## What is improving

Looking across the launch phases, the save flow has improved in three important ways:

- **Average daily save attempt success rate**
  - 40.5% -> 47.2% -> 52.1%
- **Average save-click time**
  - 5.1s -> 4.7s -> 3.8s
- **User error rate**
  - 37.4% -> 30.1% -> 24.5%

### Why that matters

This tells us the experience is getting healthier in the areas that matter most:

- more save attempts are succeeding
- successful saves are happening faster
- fewer attempts are failing because of user-side friction

### Context

- The transition period includes the **Feb 9 addition of 126 supported sites**
- The latest KPI export is refreshed through **3/19/2026**

---

## Where friction remains

### Current post-mid-Feb error mix

From the trusted post-mid-Feb period, save attempts break down roughly as:

- **52.1%** success
- **24.5%** user-side errors
- **12.6%** external/domain errors
- **10.0%** product-side errors

### What is driving the remaining friction

The two clearest friction drivers are:

1. **Unsupported sites**
2. **Users not pasting a recipe URL**

#### External/domain issues

- **92.1% of external errors are unsupported sites**  
  _(based on the detailed error breakdown export used earlier in this review cycle)_

#### User-input issues

Across all-time raw `user_input_error` rows (`n = 4,326`):

- **48.4%** blank / empty submit
- **26.3%** search terms or recipe names instead of a URL
- **10.6%** email address entered
- **7.9%** site/domain only
- **6.9%** malformed / partial URL

### What that means

This is not mainly an email problem or a broken-link problem. A large share of the user-input issue is that people are **not attempting to paste a recipe-page URL at all**.

So this is fundamentally a:

- **guidance problem**
- **validation problem**
- **site coverage problem**

not just a backend parser problem

---

## Current product focus / recommended next moves

### 1. Expand site coverage

- Continue adding the next **300+ supported sites**
- Monitor unsupported-site failures and domain concentration

### 2. Improve the input experience

- Disable save on empty submit
- Make the field explicitly about a **recipe page URL**
- Add inline validation for:
  - empty submit
  - recipe names / search terms
  - email addresses
  - site/domain-only inputs
  - malformed URLs

### 3. Keep reliability work targeted

Product-side issues are smaller than user-side and external friction, so reliability work should stay focused on:

- concentrated internal failure types
- high-volume recoverable issues

---

## Monthly site-scale context

These metrics are useful as **context**, not primary product-health KPIs.

### Successful clipper saves per 1,000 MAUs

| Month | Clipper successful saves | Total MAUs | Saves per 1,000 MAUs |
|---|---:|---:|---:|
| 12/2025* | 872 | 1,151,154 | 0.76 |
| 1/2026 | 2,272 | 1,191,231 | 1.91 |
| 2/2026 | 1,830 | 1,185,632 | 1.54 |

\*December is a partial launch month for Clipper, so it should not be compared directly with January and February.

### How to use these

These metrics help answer:

- how much Clippler is contributing within the broader MyRecipes ecosystem
- whether site-wide reach is growing

They are **not** the best standalone measure of feature health, because they use broad site-level denominators.

---

## Open questions / current data gaps

The biggest current gaps are:

- successful saves per active user
- % of active users starting a save
- repeat usage / retention
- collection creation impact
- deduped launch-to-date active-user denominator

### Current recommendation

Ask analytics for:

- launch-to-date deduped active MyRecipes users for **12/18/2025 through current date**
- weekly or monthly active-user denominators for ongoing KPI reporting

---

## Data sources

Current page is based on:

- Recipe Clipper Reporting - Data exports
- Recipe Clipper Reporting - Error Breakdown
- Recipe Clipper Reporting - Failed URLs
- MyRecipes Reporting Master Doc - Monthly Summary

---

## Monthly update template

Copy this section each month and fill in the newest period.

### [Month YYYY] update

**Headline**
- [1-2 sentence summary of what changed]

**What improved**
- [metric + movement]
- [metric + movement]

**What did not improve / where friction remains**
- [issue]
- [issue]

**Key metrics**

| KPI | Last month | Current month | Delta |
|---|---:|---:|---:|
| Add Recipe clicks |  |  |  |
| Successful saves |  |  |  |
| Save success rate |  |  |  |
| Avg save-click time |  |  |  |
| User error rate |  |  |  |
| Product error rate |  |  |  |
| External/domain error rate |  |  |  |
| External-brand share |  |  |  |

**What we are doing next**
- [action]
- [action]
- [action]

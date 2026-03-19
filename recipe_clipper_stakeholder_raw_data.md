# Recipe Clipper stakeholder raw data

Use the copy below as the source content for Google Slides or Beautify AI.

## Important definitions

- **Trusted error attribution starts 2/19/2026**
  - Save URL error categories were updated around mid-Feb, so **user vs product vs external** should be treated as cleanest from **2/19 onward**
- **Three-phase framing used in the slides**
  - **Pre-transition baseline:** **12/18-1/30**
  - **Transition period:** **1/31-2/18**
  - **Post-mid-Feb trusted period:** **2/19-3/18**
- **Avg save-click time**
  - This metric uses the shorter save-time field from the export
  - It should be labeled **avg save-click time**, not end-to-end modal time
- **Avg modal-open time**
  - Separate metric
  - Post-mid-Feb average is **10.5s**

## Slide 1 raw data

### Title
Recipe Clipper performance improved in stages

### Subtitle
A three-phase view shows improvement through the transition window and into the cleaner post-mid-Feb period.

### Core numbers to show

- **126 supported sites added on Feb 9**

- **Average daily save attempt success rate**
  - **40.5%** in **pre-transition baseline (12/18-1/30)**
  - **47.2%** in **transition period (1/31-2/18)**
  - **52.1%** in **post-mid-Feb (2/19-3/18)**

- **Avg save-click time**
  - **5.12s** in **pre-transition baseline (12/18-1/30)**
  - **4.65s** in **transition period (1/31-2/18)**
  - **3.84s** in **post-mid-Feb (2/19-3/18)**

- **External-brand share of successful saves**
  - **79.0%** in **pre-transition baseline (12/18-1/30)**
  - **79.1%** in **transition period (1/31-2/18)**
  - **85.3%** in **post-mid-Feb (2/19-3/18)**

### Supporting points

- The transition period includes the **Feb 9 addition of 126 supported sites**
- Performance improves in stages, with gains visible in the transition window and further gains in the trusted post-mid-Feb period
- Use the three-phase view for **topline KPI progression**, and use **2/19 onward** for the cleanest error attribution
- The feature is clearly being used for its intended use case:
  - the majority of successful saves are **external-brand URLs**

### What we can and cannot claim

- We **can** claim progress on:
  - save attempt success rate
  - save-click time
  - external recipe adoption
- We **cannot** claim progress on this export alone for:
  - collection creation
  - repeat sessions
  - retention

### Slide footnote
Use the three-phase view for KPI progression. Use 2/19 onward as the trusted source for user vs product vs external error attribution.

## Slide 2 raw data

### Title
Post-mid-Feb error mix shows site coverage is the main remaining scaling constraint

### Subtitle
From 2/19 onward, error attribution is the cleanest read of user vs product vs external friction.

### Outcome mix of save attempts, post-mid-Feb (2/19-3/18)

These are **average daily percentages** from the trusted post-mid-Feb period:

- **52.1% success**
- **24.5% user-side errors**
- **12.6% external/domain errors**
- **10.0% product-side errors**

### Supporting numbers

- **92.1% of external errors are unsupported-site related**
  - unsupported_site = **487**
  - site_connection_fail = **42**

- **84.2% of product errors come from general_fail + bookmark_fail**
  - general_fail = **227**
  - bookmark_fail = **114**
  - total product errors in trusted period = **405**

- **Post-mid-Feb successful saves are 85.3% external-brand URLs**
  - internal-brand share = **14.8%**
  - external-brand share = **85.3%**

### How to answer the “21%” question if it comes up

- Product issues are about **10.0% of save attempts**
- Product issues are about **21.1% of classified failures**
- Use **10.0% of attempts** on the slide headline to avoid implying the core product is broadly broken

### Strategic takeaway

- The core product is improving
- The biggest remaining scaling constraint is **site coverage**
- The next **300+ site additions** are a data-backed lever

### Recommended next moves

- add the next **300+ supported sites**
- keep reducing user-input friction
- continue targeted cleanup of **general_fail** and **bookmark_fail**

### Slide footnote
Stakeholder headline: the core product is improving, and the biggest remaining scaling constraint is site coverage rather than core product reliability.

## Optional speaker-note stats

- **Pre-transition baseline (12/18-1/30)**
  - avg success rate = **40.5%**
  - avg save-click time = **5.12s**
  - avg user error rate = **37.4% of submissions**
  - avg external-brand share = **79.0%**

- **Transition period (1/31-2/18)**
  - avg success rate = **47.2%**
  - avg save-click time = **4.65s**
  - avg user error rate = **30.1% of submissions**
  - avg external-brand share = **79.1%**

- **Immediate support-list signal**
  - avg success rate improved from **44.1%** in **Feb 1-8** to **50.3%** in **Feb 9-18**
  - avg save-click time improved from **5.17s** to **4.30s**
  - avg user error rate improved from **35.1%** to **24.9% of submissions**

- **Trusted post-mid-Feb baseline (Feb 19-Mar 18)**
  - avg success rate = **52.1%**
  - avg save-click time = **3.84s**
  - avg modal-open time = **10.5s**
  - avg product error rate = **10.0% of submissions**
  - avg user error rate = **24.5% of submissions**
  - avg external error rate = **12.6% of submissions**

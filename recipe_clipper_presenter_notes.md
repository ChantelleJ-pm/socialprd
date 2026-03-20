# Recipe Clipper presenter notes

These notes are written to sound like a PM talking through the slides, not reading a script. Use the phrasing as a guide and feel free to shorten in the room.

---

## Slide 1 - Recipe Clipper insights

### Main message
The core save experience is getting better. More save attempts are succeeding, saves are getting faster, and user-side friction is coming down.

### Suggested talk track

What I want to show on this slide is that the core save experience is moving in the right direction.

If you look across the three phases, we see improvement on the three metrics that matter most for the save flow itself.

First, average daily save attempt success rate improved from **40.5%** in the baseline period to **47.2%** in the transition period and then **52.1%** post-mid-Feb. So we are up **11.6 points** from baseline.

Second, average save-click time improved from **5.1 seconds** to **4.7 seconds** to **3.8 seconds**. So the experience is not just converting better, it is also getting faster. That is about **1.3 seconds faster than baseline**, or roughly **25% faster**.

Third, user error rate improved from **37.4%** to **30.1%** to **24.5%**. So user-side friction is down **12.9 points**, which is about **35% lower than baseline**.

The short version is: more people are getting through the save flow successfully, and they are doing it faster, with less user-side failure.

The other thing I would call out is that this improvement happened in stages, not all at once. The transition period includes the **Feb 9 addition of 126 supported sites**, so that is one likely contributor to the improvement we see in the middle and then the cleaner post-mid-Feb period.

And just as context, external-brand share reached **85.3%** post-mid-Feb, which is a good sign that the feature is being used for its intended job: clipping recipes from outside MyRecipes, not just circulating owned content.

### Short version if you need to move faster

This slide is basically the headline that the save flow is healthier now than it was at launch. Success rate is up, save-click time is down, and user error rate is down. So both conversion and experience quality are improving.

### Good transition to Slide 2

So Slide 1 is the good news: the flow is improving. Slide 2 is really about what is still getting in the way, and where we should focus next.

### If someone asks about the math

**Why ~25% faster?**

We are comparing **5.12 seconds** in baseline to **3.84 seconds** post-mid-Feb.

That is:

- **1.28 seconds faster**
- or **(5.12 - 3.84) / 5.12 = 25%**

So saying **~25% faster** is just the relative improvement from baseline.

**Why ~35% lower?**

We are comparing user error rate:

- **37.4%** baseline
- **24.5%** post-mid-Feb

That is:

- **12.9 percentage points lower**
- or **(37.4 - 24.5) / 37.4 = 34.5%**

So we rounded that to **~35% lower**.

---

## Slide 2 - Remaining friction / what is still driving failure

### Main message
At this point, the biggest remaining friction is not core product reliability. It is mainly a combination of unsupported sites and users not pasting a recipe URL.

### Suggested talk track

What I want to do on this slide is shift from overall performance to what is still causing failure.

At the top, the post-mid-Feb save-attempt mix shows that about **52.1%** of attempts are successful, while the remaining failures are split across **user-side issues**, **external/domain issues**, and **product-side issues**.

The important takeaway here is that the biggest remaining friction is **not primarily core product reliability**.

User-side issues are the largest remaining bucket at **24.5% of attempts**, and external/domain issues are the next largest at **12.6%**. Product-side issues are smaller at **10%**.

Then if we look one layer deeper, the clearest external pattern is that **92.1% of external errors are unsupported-site related**. So within the external bucket, unsupported coverage is doing most of the work.

And when we look at the detailed Failed URLs data, the clearest user-side pattern is that people often are not pasting a recipe URL at all.

Nearly half of user-input errors are **blank submits**, and another **26.3%** are recipe names or search terms instead of a URL. So this is not mainly an email problem or a malformed-link problem. It is more fundamentally a guidance and validation problem.

That is why the recommended next moves are:

- expand the next **300+ supported sites**
- reduce empty and non-URL submissions
- add paste-first guidance and inline validation

So the framing I would use is: the save flow is improving, but the next unlock is less about major core-product repair and more about **site coverage plus a clearer input experience**.

### Short version if you need to move faster

This slide says the main remaining friction is outside pure product reliability. On the external side, unsupported sites are the dominant issue. On the user side, a lot of people are not pasting a recipe URL at all. So the next improvements should focus on site coverage and input guidance.

### Good closing line

If I were to simplify the whole story into one sentence, it would be: **the experience is healthier than it was at launch, and the next gains will come from better coverage and better input guidance rather than just backend fixes.**

### If someone asks why the slide mixes timeframes

The top chart uses the **post-mid-Feb** period because that is the cleanest window for error attribution.

The lower user-input chart uses **all-time raw Failed URLs data** because for that question, we are already inside the `user_input_error` bucket and directly looking at what people typed or pasted. So for understanding user behavior, using the full raw data gives us the better sample and the same overall pattern.

### If someone asks whether the title is too strong

A softer version would be:

**The two clearest remaining friction drivers are unsupported sites and users not pasting a recipe URL.**

That is the safest way to say it.

---

## Optional opener for the whole section

I am going to spend just a couple of minutes on two things: first, how the core save experience is trending, and second, what still seems to be driving the remaining friction.

---

## Optional closer for the whole section

So overall, I think the story is encouraging: the core save flow is improving, and the remaining problems are specific enough that we can act on them. The next step is less about broad diagnosis and more about focused execution on site support and input UX.

---

## Fast backup answers

### Why not use external-brand share as a top KPI on Slide 1?
Because it tells us more about intended use and product fit than about raw save-flow performance. For a performance slide, success rate, speed, and user error rate are stronger.

### Are user-input errors mostly bad URLs?
No. The biggest patterns are blank submits and recipe names or search terms instead of a recipe URL.

### Are emails a big problem?
They happen, but they are not the main issue. They are much smaller than blank submissions and search-type behavior.

### Does this prove repeat usage or retention?
No. These exports are good for save-flow performance and friction, but not enough to prove downstream behavior like repeat use or retention.

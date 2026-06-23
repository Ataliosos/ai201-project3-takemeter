# TakeMeter — Planning Document

> Written before data collection. Updated before stretch features.

---

## Community

I chose Reddit career and IT communities — specifically r/cscareerquestions, r/ITCareerQuestions, and r/cybersecurity.

Growing up as a first-generation college graduate from an immigrant family, my family had no experience with technology careers, cybersecurity, or IT. I had no one to ask about resumes, certifications, or how to break into the field. Online communities like these were where I actually learned how any of it worked. People shared real advice, real failures, and real opinions — and the quality of those posts varied a lot.

That variation is exactly what makes this a good classification task. Some posts give you specific, actionable steps you can take today. Some tell a personal story about someone's journey into tech. Some ask genuine questions looking for guidance. And some are just strong opinions stated confidently with no real backing. These categories are distinct enough to label reliably, but they come up constantly in these communities — meaning 240 examples is very reachable and the model will have real signal to learn from.

This also matters to people who actually use these communities. A first-gen student who finds a post titled "How I broke into cybersecurity" wants to know: is this person giving me steps I can follow, or are they just telling their story? Those are different things and they're worth distinguishing.

---

## Labels

### `actionable_advice`

**Definition:** A post whose primary purpose is giving specific recommendations, instructions, or steps the reader can act on — not just sharing what happened to the author.

**Example 1 (clear):**
> "Tailor your resume for every job application. Use the exact keywords from the job posting in your skills section. Recruiters use ATS filters and a generic resume gets screened out before a human sees it."

*Why it fits:* Specific steps, directed at the reader ("your resume"), tells you what to do and why.

**Example 2 (clear):**
> "Before applying for help desk roles, practice Active Directory, ticketing systems like ServiceNow, and basic networking. Set up a home lab with VirtualBox — it costs nothing and gives you something to talk about in interviews."

*Why it fits:* Concrete tools named, specific action (home lab), reader-directed throughout.

---

### `personal_experience`

**Definition:** A post whose primary purpose is sharing the author's own career journey, outcome, or lesson — not telling the reader what to do.

**Example 1 (clear):**
> "I applied to over 50 jobs before getting my first interview. Six months of rejections, then one callback that turned into an offer. It was the most demoralizing experience of my career but I learned what not to do."

*Why it fits:* Author's story, past tense, focuses on what happened to them — not instructions for the reader.

**Example 2 (clear):**
> "My internship taught me more practical skills than four years of classes combined. I didn't know how to read a real ticket until I sat next to someone doing it."

*Why it fits:* Personal reflection, author's outcome, not advice-giving.

---

### `hot_take`

**Definition:** A strong opinion stated confidently with little or no supporting evidence. The post asserts a position — often a controversial one — but doesn't back it up with data, research, or detailed reasoning.

**Example 1 (clear):**
> "Certifications matter more than college degrees for IT jobs. A CS degree doesn't teach you anything practical. A Security+ will get you further than four years of tuition debt."

*Why it fits:* Bold claim, no data cited, no evidence that would survive scrutiny — just a confident assertion.

**Example 2 (clear):**
> "Most entry-level job postings are fake. Companies post them to collect resumes and never intend to hire externally. The whole system is rigged against new graduates."

*Why it fits:* Sweeping claim ("most"), no evidence, framed as obvious fact.

---

### `question_request`

**Definition:** A post whose primary purpose is asking for information, clarification, or guidance from the community.

**Example 1 (clear):**
> "How do I get my first cybersecurity internship with no experience? I'm finishing my sophomore year with a 3.4 GPA and I have CompTIA A+ but nothing else on my resume."

*Why it fits:* Asking, not telling. Provides context to help others answer.

**Example 2 (clear):**
> "Is Security+ worth earning before graduation or should I wait until I have a job lined up? Trying to figure out how to prioritize my time this semester."

*Why it fits:* Direct question, seeking community input, no opinion or story being shared.

---

## Hard Edge Cases

### Edge case 1: `personal_experience` vs `actionable_advice`

**Example post:**
> "I got my first IT job after spending three months building projects on GitHub, so everyone should build projects instead of just studying for certs."

**Why it's hard:** The post starts as a personal story (what worked for me) but ends as a directive (what everyone should do).

**Decision rule:** What is the *main purpose* of the post — telling the reader what happened to the author, or telling the reader what to do?

If the story is the vehicle for delivering advice and most of the post is recommendation → `actionable_advice`.
If the advice is almost incidental and most of the post is the story → `personal_experience`.

**Decision for this example:** The post is one sentence of story and one directive. The directive is the point. → `actionable_advice`

**Contrast:** "I spent three months building GitHub projects and landed my first IT job. The hardest part was figuring out which projects to build — I started with a simple network monitor and worked up from there. The interview went well because I could actually talk through my code." → `personal_experience` (the story is the whole post, no directive to the reader)

---

### Edge case 2: `hot_take` vs `actionable_advice`

**Example post:**
> "Stop wasting time on CCNA. Network+ is enough for 90% of entry-level roles and takes half the time to study for. I see too many people delay job searching because they're chasing Cisco certs nobody asked for."

**Why it's hard:** Sounds like advice ("stop doing X, do Y instead") but has no evidence — just the author's opinion stated forcefully.

**Decision rule:** Does the post provide specific, checkable reasons for the recommendation, or is it just asserting what to do?

If the reasoning is evidence-based and someone could verify or challenge it → `actionable_advice`.
If the recommendation is just stated confidently with opinion as the only backing → `hot_take`.

**Decision for this example:** "90% of entry-level roles" is an unverified claim. No job posting data, no source. The whole post is an opinion dressed as advice. → `hot_take`

---

### Edge case 3: `question_request` vs `personal_experience`

**Example post:**
> "I've been job searching for 8 months and I'm starting to lose hope. Has anyone else been through this? How did you stay motivated?"

**Why it's hard:** Shares personal situation (experience signal) but ends with a question (question signal).

**Decision rule:** Is the question the main purpose, or is sharing the situation the main purpose?

If the post is primarily venting or narrating and the question is almost an afterthought → `personal_experience`.
If the post is primarily seeking community input and the context is just setup → `question_request`.

**Decision for this example:** The post ends with a direct question asking others for advice and experiences. The context (8 months, losing hope) is setup for the question, not the point of the post. → `question_request`

---

## Data Collection Plan

**Sources:**
- r/cscareerquestions (top posts, week/month, and new posts)
- r/ITCareerQuestions
- r/cybersecurity (career-related posts only)

Using the public Reddit JSON API — no authentication required:
- `https://www.reddit.com/r/cscareerquestions/top.json?limit=100&t=month`
- `https://www.reddit.com/r/ITCareerQuestions/new.json?limit=100`

**Target distribution:**

| Label | Target | % |
|---|---|---|
| `actionable_advice` | 60 | 25% |
| `personal_experience` | 60 | 25% |
| `hot_take` | 60 | 25% |
| `question_request` | 60 | 25% |
| **Total** | **240** | **100%** |

Even split across all four labels — no label should dominate. 25% each keeps the model from learning to predict the majority class.

**Where to find each label:**
- `actionable_advice`: posts with titles like "Tips for...", "Guide to...", "What you should do before..."
- `personal_experience`: posts with "I got my first job", "My journey", "6 months later..."
- `hot_take`: controversial posts, posts with "unpopular opinion", strong declarative titles
- `question_request`: posts ending in "?" or starting with "How do I", "Is X worth it", "Should I"

**If a label is underrepresented after 180 examples:** Search that subreddit specifically for that type. For `hot_take`, try the controversial sort. For `personal_experience`, search "my experience" or "my story." Collect until balanced before labeling.

**CSV format:**
```
text, label, notes
"Tailor your resume for every job...", actionable_advice, ""
"I applied to 50+ jobs before...", personal_experience, ""
"Certs matter more than degrees...", hot_take, ""
"How do I get my first internship...", question_request, "groq_prelabel - reviewed"
```

---

## Evaluation Metrics

I will use the following metrics and report them for both models (fine-tuned and baseline):

| Metric | Why I'm using it |
|---|---|
| **Overall accuracy** | Quick summary of how often the model is right across all four labels |
| **Per-class F1** | The most important metric — tells me how the model performs on each label individually, not just in total |
| **Precision and Recall per class** | Precision = when the model predicts a label, how often is it right. Recall = of all the real examples of a label, how many did the model catch |
| **Confusion matrix** | Shows exactly which labels are being mixed up and in which direction |

**Why accuracy alone isn't enough:** If `question_request` posts are slightly more common in the data, a lazy model that predicts `question_request` for everything would still score 25%+ without learning anything useful. Per-class F1 catches this immediately — that model would get F1=0 for the other three labels.

**Why F1 specifically:** F1 balances precision and recall into one number per class, which is useful when I want to compare models without having to read a full table every time.

---

## Definition of Success

**Minimum bar (model is usable):**
- Overall accuracy ≥ 70%
- Per-class F1 ≥ 0.60 for every label — no label completely ignored
- No label with recall below 50%

**Target performance (model is genuinely useful):**
- Overall accuracy ≥ 78%
- Per-class F1 ≥ 0.70 for all four labels
- Fine-tuned model beats the Groq zero-shot baseline by at least 10 percentage points

**Why these numbers:** A model that gets 70% accuracy and 0.60 F1 per class could realistically be used as a post-tagging assistant in a career community — surfacing actionable advice for people who need steps, not stories. Below that threshold, the error rate makes it more misleading than helpful. A first-gen student who gets sent `hot_take` posts when they need `actionable_advice` is worse off than before.

**Red flag:** If fine-tuned accuracy exceeds 95%, I'll check for data leakage or whether the model learned surface features (question marks → question_request, "I" → personal_experience) instead of actual discourse quality.

---

## AI Tool Plan

### Label stress-testing (before annotation)

**Tool:** Claude and ChatGPT

**What I'll give it:** My four label definitions and the three edge cases above.

**Task:** Generate 10–15 posts that sit at the boundary between two labels — especially `personal_experience` vs `actionable_advice` (my hardest pair) and `hot_take` vs `actionable_advice`.

**How I'll verify:** I'll try to label every generated post using my decision rules. If I can't cleanly classify more than 2 of them, the definition needs tightening. I'll revise and re-test before annotating 240 examples.

---

### Annotation assistance (during data collection)

**Tool:** Groq llama-3.3-70b-versatile

**What I'll give it:** My four label definitions + a batch of 50 unlabeled posts at a time.

**Task:** Pre-label each post as one of {actionable_advice, personal_experience, hot_take, question_request} with a one-sentence explanation.

**My review process:** I will read every pre-labeled post myself and decide whether I agree. I will not accept any label I didn't personally verify. Posts where I disagreed with the pre-label will be flagged in the `notes` column as "groq_prelabel_corrected."

**Disclosure:** All AI-assisted labels will be disclosed in the README AI usage section.

---

### Failure pattern analysis (after fine-tuning)

**Tool:** Claude

**What I'll give it:** All wrong predictions from the test set — post text, true label, and predicted label.

**Task:** Identify any patterns — do errors cluster around a specific label pair? Are errors concentrated in short posts or posts about a specific topic? Is there a linguistic feature (first-person language, question marks, confident declarative sentences) that correlates with errors?

**How I'll verify:** I'll manually re-read every example in any pattern the AI identifies and check whether the pattern holds. I won't include a pattern I can't verify from the data itself.

---

## Architecture Diagram

```
Reddit post (text input)
         │
         ▼
┌─────────────────────────────────────────────┐
│           TakeMeter Classifier              │
│                                             │
│  distilbert-base-uncased tokenizer          │
│  → token IDs + attention mask               │
│                                             │
│  Fine-tuned DistilBERT (6 transformer layers│
│  + classification head: 768 → 4 logits)     │
│                                             │
│  Softmax → probabilities                    │
│  argmax  → predicted label                  │
└──────────────┬──────────────────────────────┘
               │
      ┌────────┴────────┐
      ▼                 ▼
predicted label    confidence score
(actionable_advice /    (0.0–1.0)
 personal_experience /
 hot_take /
 question_request)
      │
      ▼
Gradio UI (stretch feature)
displays label + confidence bar

─────────────────────────────────────────────

Training pipeline:

labeled_data.csv (240 examples)
         │
         ▼
   70/15/15 split
   168 train / 36 val / 36 test
         │
         ▼
   Google Colab T4 GPU
   distilbert-base-uncased
   3 epochs, lr=2e-5, batch=16
         │
         ├──► evaluate on test set
         │    accuracy, F1, confusion matrix
         │
         └──► compare vs Groq zero-shot baseline
              (same 36-example test set)
```

---

## Complete Interaction Walkthrough

**Input post:** "Before applying for help desk roles, get comfortable with Active Directory, a ticketing system like ServiceNow, and basic networking concepts. Set up a home lab — VirtualBox is free and having something real to walk through in an interview makes a huge difference."

**Step 1:** Text is tokenized by distilbert-base-uncased → token IDs tensor.

**Step 2:** Token IDs pass through 6 transformer layers → [CLS] token hidden state (768-dim).

**Step 3:** Classification head: 768 → dropout → 4 logits.

**Step 4:** Softmax: `{actionable_advice: 0.83, personal_experience: 0.07, hot_take: 0.05, question_request: 0.05}`.

**Step 5:** Predicted label = `actionable_advice`. Confidence = 0.83. ✅ Correct.

**Error path:** "I got my first help desk job after building a home lab" — true label `personal_experience`. A model that over-learned "home lab" → `actionable_advice` from training data might misclassify this. That's the kind of surface-feature error I'll look for in failure analysis.

---

## Stretch Feature Plans

**Inter-annotator reliability (+1pt):** Have a classmate or friend label 30 posts using only the four definitions — no edge case rules. Calculate Cohen's kappa. Analyze where we disagreed.

**Confidence calibration (+1pt):** Bucket test predictions by confidence level. Check whether higher confidence predictions are actually more accurate.

**Error pattern analysis (+1pt):** Use Claude to identify systematic patterns in wrong predictions, then verify every pattern manually before including it in the report.

**Deployed interface (+1pt):** Gradio app that takes a post as input and returns the predicted label + confidence score. Document how to run it in the README.
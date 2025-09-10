# AI Engineer Home Test — YouTube Advice Chatbot

## Overview

Build a small but production-minded chatbot that gives creators practical advice on how to improve their YouTube channel, grounded **only** in two provided video transcripts.  Answers must include citations pointing to the specific video(s) and timestamp ranges used to generate the advice when applicable.

* **Timebox (suggested):** 4–6 hours
* **What we provide:** A `transcripts/` folder containing two \~1-hour transcripts about YouTube channel growth, each with timestamps and basic metadata.

  * `aprilynne.txt` This video is about improving video introductions.
  * `hayden.txt` This video is about improving storytelling.
* **What you deliver:** A runnable project with a README and a minimal interface (CLI or simple web/API endpoint) that we can use to ask questions and view grounded answers with sources.

> You may use any code generation tools (e.g., Copilot, ChatGPT, Cursor, Claude Code) but you should be prepared to explain **every** part of your code and design decisions during the review.

---

## Core Requirements

1. **Grounded Answers with Citations**

   * Responses must cite the relevant video(s) and timestamp ranges. Choose a clear, machine-checkable format, e.g.:

     * `[source: video_1 t=00:12:34–00:13:10]`
     * or `[source: "<Video Title 1>" t=12:34–13:10]`
   * If multiple passages inform the answer, include multiple citations.
   * If the transcripts do not contain the needed info, say so and (optionally) ask a clarifying question.

2. **Interface (no full frontend required)**

   * Provide **one** of:

     * A CLI command, e.g. `python chat.py --q "How do I improve click-through rate?"`
     * A simple local web endpoint, e.g. `POST /ask {"question": "..."}` printing JSON
     * A short notebook with a `ask(question: str)` function and sample cells
   * We should be able to run locally and see both the advice and its citations.

3. **Use Only the Provided Transcripts for Grounding**

   * You may use a general-purpose LLM, but the **facts and recommendations** must be grounded in the provided transcripts. Do not pull in other sources for content unless clearly marked as opinion/speculation **and** kept separate from transcript-grounded advice.

4. **Reproducibility**

   * Include a `requirements.txt` or `pyproject.toml` (or equivalent for your stack).
   * Pin model/library versions when possible. If using an API, note versions/models.
   * Provide a seed or notes to obtain similar results across runs.

5. **Performance & Cost Awareness (lightweight)**

   * Keep latency and token usage reasonable. A short note in the README about expected runtime/cost per query is sufficient.

---

## Deliverables

**Please submit a single repository** (zip or link) with the following structure (flexible, but clarity helps):

```
.
├─ README.md                # how to run & test
├─ DESIGN.md                # (brief) architecture & tradeoffs
├─ requirements.txt         # or pyproject/pnpm/etc
├─ src/                     # code to run your submission
├─ tests/.                  # tests and evals
└─ transcripts/
   ├─ video_1_transcript.txt
   └─ video_2_transcript.txt
```

### README.md (required)

Include:

* **Setup:** environment, dependencies, API keys (if any), and how to obtain them.
* **Run:** clear commands to start the interface and ask a question.
* **Test:** how to run your tests and the evaluation harness.
* **Model details:** which model(s) you used and why.
* **Limits:** known limitations and future improvements.

### Tests & Minimal Evaluation Harness

Provide at least:

* **Schema test:** Asserts that every answer includes one or more citations in the agreed format.
* **Grounding test:** Given a known question whose answer appears only in `video_2`, verify at least one citation points to `video_2`.
* **Fallback test:** For a question outside the transcripts, verify the system gracefully declines with a helpful message.

A simple `eval.py` that runs a small set of prompts and prints results (and pass/fail) is sufficient. Example prompts are provided below.

---

## Response Format (what your bot should return)

Your bot’s answer for each question should include:

1. **Actionable recommendations** tailored to the question (bulleted).
2. **Rationale** for each recommendation (one short sentence each is fine).
3. **Citations** immediately after the relevant bullet or grouped at the end.

**Example**

```
Q: How can I improve my thumbnails and CTR?

A:
- Use bold, high-contrast faces with a single focal point; avoid tiny text. [source: video_1 t=00:12:34–00:13:10]
- Test 2–3 variants before publishing using community polls. [source: video_2 t=00:45:02–00:46:18]
- Keep title and thumbnail complementary, not redundant. [source: video_1 t=00:27:11–00:28:05]

If you want, I can suggest A/B test ideas based on your recent uploads.
```

> The above is **illustrative only**. Your actual answers must be grounded in the provided transcripts.
> You may use whatever formatting you like as long expected elements are present. 

---

## Acceptance Criteria (we will check)

* ✅ **Citations present and correct:** Every nontrivial claim links to at least one transcript citation; IDs/timestamps resolve back to the correct file/segment.
* ✅ **Useful & actionable advice:** The output is specific and implementable, not generic.
* ✅ **Groundedness:** Content demonstrably comes from the transcripts; hallucinations are minimized.
* ✅ **Robustness:** Handles ambiguous/out-of-scope questions gracefully.
* ✅ **Reproducibility:** We can install, run, and test without guesswork.

---

## Evaluation Rubric (100 pts)

* **Usefulness & Correctness (35 pts):** Are recommendations actionable and aligned with the transcripts?
* **Grounding & Citations (25 pts):** Are sources precise, with correct video and timestamps?
* **Engineering Quality (20 pts):** Clear structure, readable code, sane abstractions, minimal but appropriate error handling.
* **Reproducibility (10 pts):** Setup docs, deterministic configs, easy local run.
* **Communication (10 pts):** DESIGN.md clarity, tradeoffs explained, constraints acknowledged.

> We may ask you to walk through your code and discuss design choices during the review.

---

## Notes

* You may preprocess transcripts (e.g., normalization, sentence splitting) as needed.
* Keep the footprint modest; don’t over-engineer. A working, well-explained baseline beats an unfinished system.

---

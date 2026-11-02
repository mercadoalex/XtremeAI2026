# Talk Outline: The Socratic Method in AI, a Tale from the Trenches

**Speaker:** Alejandro Mercado · **Event:** XtremeAI 2026 · **Slot:** 20 min + 5 min Q&A · **Language:** English
**Case:** Blaze Platform (BlazeBPF, BlazeCOBOL, BlazeFortran): an AI-based learning platform that is agnostic to the technology being taught.

---

## 1. The one-sentence thesis

> **Don't let the model guess what happened. Observe it, let the model interpret it, and let a Socratic tutor ask the student the right question about it.**

Everything in the talk serves this sentence.

## 2. The unified vision

Three pieces that are usually built separately become one dialogue:

| Piece | Role | Source in code (for your own reference, not for slides) |
|---|---|---|
| **Observe** | eBPF probes capture what the student's program really did (syscalls, performance, memory, security). The result is a typed `ObservationResult`. | `cobol-ebpf-observability` spec, `shared-types/observation`, `native/ebpf/*.bpf.c` |
| **Explain** | The model (served via Amazon Bedrock) interprets the observation, grounded in source code + observed data. | `ai-explainer.ts` |
| **Ask** | The Learning Strand Agent runs the Socratic dialogue: progressive hints, learner profile (mastered / struggling concepts, success rate, time), conversation memory. | `learning-strand-agent.ts` |

The loop:

```text
Student writes code
      |
[1 OBSERVE]   eBPF probes + sandbox  ->  ObservationResult (facts)
      |
[2 EXPLAIN]   The model, grounded in source + ObservationResult
      |
[3 ASK]       Strand Agent: predict / probe / hint, adapted to the learner profile
      |
Student answers, changes code
      |
[1 OBSERVE]   measure again -> compare before / after   (the loop closes)
```

Why it matters: **the facts come from the kernel, not from the model.** The model never has to guess what the program did.

## 3. Status labels used on the slides

Use one honest status line, not a disclaimer. Suggested wording for the architecture slide:

- **Built:** observation pipeline, `ObservationResult` contract, AI explainer, Socratic strand agent with learner profile and hint ladder (in BlazeBPF), and a lighter learning companion in BlazeCOBOL.
- **Connecting now:** feeding the observed facts into the Socratic companion so the questions are about what was measured. Work brief: `socratic-observation-bridge.md`.

Only claim in the talk what is true on Nov 24. If the bridge is demoed live or from a recording, say so; if it is still in progress, present it as "connecting now".

Say it with confidence: "These pieces exist. We are wiring them into one dialogue."

## 4. Narrative arc

1. **Trust problem** (glass house): building on a model that may hallucinate.
2. **Turn:** "Answers are cheap. Understanding isn't."
3. **Idea:** replace answering with asking, but ask about **facts**.
4. **System:** observe, explain, ask, re-observe.
5. **Personalization:** the tutor reads *how* you answer (correctness, time, error type) and picks the next step. Not a linear "next, next, next" syllabus.
6. **Lessons from the trenches.**
7. **Pattern to copy on Monday:** Predict, Observe, Compare, Explain.

## 5. Slide plan (20 minutes)

| # | Time | Slide | Content | Notes |
|---|---|---|---|---|
| 1 | 0:00 | Title | Talk title, name | Done |
| 2 | 0:30 | Speaker | Photo, credentials | Done |
| 3 | 1:00 | Glass house | "Would you build a glass house during an 8.0 earthquake?" | From your earlier talk, translated |
| 4 | 1:45 | The weight of distrust | Fear of building on a hallucination | Short |
| 5 | 2:30 | Transition | **"Answers are cheap. Understanding isn't."** then "What if the AI asked instead of told?" | Agreed |
| 6 | 3:15 | Why LLMs hallucinate + why questions help | Probability vs truth; grounding; one line of research (Socratic prompting, debate, Chain-of-Verification) | One slide, not three |
| 7 | 5:30 | Blaze Platform | Agnostic to the technology: eBPF, COBOL, Fortran. Shared contract, vertical probes | `[TODO: platform URL]` |
| 8 | 7:00 | The thesis | Observe, interpret, ask | The sentence from section 1 |
| 9 | 8:00 | Observe | eBPF probes + sandbox, `ObservationResult` (trimmed example) | Show a real, trimmed JSON |
| 10 | 10:00 | Explain | The model grounded in source + observation; low temperature; prompt constraints | Show 6 to 8 lines of the prompt |
| 11 | 11:30 | Ask | Strand agent: hint ladder, learner profile, adaptive path | Diagram of the profile |
| 12 | 13:30 | The unified loop | The diagram from section 2 with the status line | Core slide |
| 13 | 14:30 | Example dialogue | One full exchange using all three pieces | See section 6 |
| 14 | 16:00 | Trenches | Invented numbers, explainer that doesn't ask, noisy signals (time), privacy | See section 7 |
| 15 | 18:00 | Pattern for Monday | **Predict, Observe, Compare, Explain** | Replaces the generic prompt-template slide |
| 16 | 19:00 | Takeaways | 3 to 4 bullets | |
| 17 | 19:30 | References | Quick or skip | |
| 18 | 19:30 | Q&A | Links | Done |

This replaces the earlier 16-slide deck. Slides 1, 2 and 18 already exist; the research slide is condensed; the generic architecture and dialogue slides are replaced.

## 6. Illustrative dialogue (mock, replace with a real transcript when available)

Context: a COBOL student reads a file one record at a time.

- **Student:** My program is slow when it reads the file. Why?
- **Observe (facts):** `read` called 1,200 times, 1 byte each. *(illustrative numbers)*
- **Tutor (Ask):** Before I show you what I measured: how many times do you think your program asks the kernel to read from the file?
- **Student:** Maybe a few dozen?
- **Tutor:** The probes counted 1,200 calls of 1 byte each. What does that tell you about how the file is being read?
- **Student:** It's reading byte by byte?
- **Tutor (Explain):** Right. Each call crosses into the kernel. What could you change so that fewer calls are needed?
- **Student changes code, platform observes again:** `read` called N times. *(the measured result, not an estimate)*
- **Tutor:** Compare the two runs. What changed, and why?

This one exchange uses all three pieces: facts from eBPF, grounded interpretation from the model, and a Socratic sequence adapted by the profile.

## 7. Lessons from the trenches (use only what is true for you)

Candidates already visible in the design or code. Keep the ones that match your experience:

- **The model should not invent numbers.** The observation gives facts; predictions like "reduces calls by 90 %" are the model's guess. Fix: cite only figures present in the data, compute derived metrics in code, label projections as estimates, and verify by re-observing.
- **Explanation vs questioning.** A one-pass explainer answers; a tutor asks. They need to be one dialogue.
- **Time is a noisy signal.** A slow answer may mean distraction, not difficulty.
- **Running student code is a security problem.** Sandbox, namespaces and resource limits.
- **Fallbacks:** if the AI fails or times out, the system must degrade gracefully.
- `[TODO: one real war story from your own experience]`

## 8. The pattern to copy on Monday

```text
1. PREDICT   Ask the learner what they expect to happen.
2. OBSERVE   Measure what really happened (not the model's opinion).
3. COMPARE   Ask what differs between prediction and reality.
4. EXPLAIN   Only now let the model explain, grounded in the measurement.
```

It works for any technology you can measure, which matches the platform being technology-agnostic.

## 9. Open items

- [ ] Confirm which vertical to demo (BlazeCOBOL suggested).
- [ ] Connect `ObservationResult` into the strand agent (the "connecting now" piece).
- [ ] Update the explainer prompt: only cite numbers from the data, label estimates.
- [ ] Get one real transcript and one trimmed `ObservationResult` for slides 9 and 13.
- [ ] Decide the public name and URL for the platform.
- [ ] Confirm what code and diagrams can be shown publicly (the repos are private). Keep bucket names, infrastructure config and internal paths out of the slides.
- [ ] Remove all employer references.
- [ ] Rewrite `script.md` and `index.html` from this outline once approved.

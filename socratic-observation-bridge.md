# Brief: Socratic Observation Bridge

**Repo:** `mercadoalex/blaze-platform` (private) · **Target date:** before Nov 24, 2026 (conference demo)
**Audience:** developers working in Kiro. This file is written to be used as the starting point of a Kiro spec.

## How to use this in Kiro

1. Copy this file to the repo, for example `.kiro/specs/socratic-observation-bridge/brief.md`.
2. In Kiro, start a new **Spec** session and say: *"Create a spec (requirements, design, tasks) from #File brief.md. Keep the scope to Phase 1 first."*
3. Review the generated requirements and design against sections 3 to 7 below, then run the tasks.
4. Each phase below is independently shippable. Phase 1 is the only one needed for the demo.

If you prefer a vibe session instead: open the files listed in section 2 with `#File` and ask Kiro to implement Phase 1 task by task.

---

## 1. Goal

Make the **Observe**, **Explain** and **Ask** pieces work as one dialogue:

```text
Student writes code
   |
[OBSERVE]  eBPF probes + sandbox  ->  ObservationResult      (facts, already built)
   |
[DIGEST]   deterministic summary of the facts (NEW)
   |
[ASK]      Learning Companion asks Socratic questions about the digest (NEW)
   |
Student answers / changes code
   |
[OBSERVE]  run again -> compare previous vs current digest (NEW)
   |
[EXPLAIN]  only now, if needed, the model explains (existing explainer, changed to run on demand)
```

Thesis to implement: **the model never guesses what the program did. Facts come from the kernel; the model interprets and asks.**

### Demo scenario (acceptance target)

A COBOL program reads a file one byte at a time.

1. Student runs "Compile & Observe".
2. Before showing the numbers, the companion asks the student to **predict** how many times the program asks the kernel to read.
3. The platform shows the measured facts (e.g. read call count). The companion asks the student to **compare** prediction vs reality.
4. Student changes the code and runs again. The companion asks what changed, citing **measured before/after** values.
5. Only then (or if the student is stuck) the model explains.

---

## 2. Current state (from a partial read of the repo; verify before relying on it)

| Area | Where | What it does today |
|---|---|---|
| Observation contract | `packages/shared-types/src/observation/index.ts` | `ObservationResult` (syscalls, syscallSummary, performance, memory, security, limits, optional `aiExplanation`). Also a cross-vertical `ObserveReport` in `observe-report.ts`. |
| Observe pipeline (shared) | `packages/shared-backend/src/observe/ObserveServiceBase.ts` | Abstract base: validation, bounded concurrency (5 active, 20 queued), sandbox, probes, report assembly. Verticals extend it. |
| Observe (vertical) | `apps/blazecobol/backend/src/services/` (`observe.ts`, `COBOLObservationManager.ts`, `observation-store.ts`, `observe-validation.ts`) | COBOL compile, sandbox, eBPF probes (`native/ebpf/*.bpf.c`), DynamoDB persistence. `apps/blazefortran` has files with the same names. |
| Explainer | `apps/blazecobol/backend/src/services/ai-explainer.ts` | One-shot explanation from COBOL source + a JSON summary of `ObservationResult`. Temperature 0.2, 30 s timeout, returns `null` on failure. |
| Companion (COBOL) | `apps/blazecobol/backend/src/services/strand-routes.ts` | **Simplified** `/api/strand/interact`: friendly hints/explanations/examples, `temperature 0.4`. Not Socratic. Reads only `context.currentChallenge` and `context.currentExercise`. `GET /profile/:userId` is a **hard-coded stub**. |
| Full Socratic agent | `apps/blazebpf/backend/src/ai/learning-strand-agent.ts` | Interaction types (hint, explanation, stuck, success, concept), up to 5 hint levels from `attemptCount`, learner profile (mastered/struggling concepts, success rate, avg time, preferred hint level, motivation), persisted conversation history with summarization, content filter. Not wired to `ObservationResult`. |
| UI | `packages/shared-ui` (`LearningStrandPanel`, `observe/ObserveReportModal`), `apps/blazecobol/frontend` (`ObservationViewer`) | Companion panel and observation viewer exist separately. |

**Key finding:** the Socratic behaviour with profile and hint ladder lives in the BlazeBPF agent, while the COBOL companion is a lightweight, non-Socratic version with a stub profile. The bridge has to deal with this (see task 3).

---

## 3. Design

### 3.1 ObservationDigest (new, deterministic, no LLM)

Create a pure function that turns an `ObservationResult` into a small, bounded summary. **Every derived number is computed here, in code, never by the model.**

```ts
// suggested location: packages/shared-types/src/observation/digest.ts (type)
//                     packages/shared-backend/src/observe/buildObservationDigest.ts (function)
export interface ObservationDigest {
  sessionId: string;
  compilationSuccess: boolean;
  executionDurationMs: number;
  syscallTotals: { totalCalls: number; distinct: number };
  topSyscalls: Array<{ name: string; count: number; totalElapsedUs: number }>; // top 5 by count
  hotSections: Array<{ name: string; durationMs: number; pctOfTotal: number }>; // top 3
  memory: { peakUsageBytes: number; totalAllocatedBytes: number; totalFreedBytes: number; unfreedBytes: number };
  security: { flaggedCount: number; findings: Array<{ syscall: string; severity?: string; detail: string }> }; // max 5
  limits: { resourceLimitExceeded: boolean; limitDetail?: string };
  facts: string[]; // human-readable facts generated by code, max ~8, the ONLY source of numbers the tutor may cite
}
```

Rules:
- Bounded size (cap arrays, truncate strings) so the prompt stays small.
- `facts` examples: `"read was called 1200 times (total 340 ms)"`, `"peak memory 24 KB; 0 bytes unfreed"`.
- Do not include raw `syscalls[]`, flame graph data or full source in the digest.
- Prefer the cross-vertical `ObserveReport` as the long-term input so Fortran and eBPF verticals can reuse it. For the demo, building the digest from `ObservationResult` is enough. Decide and document in the design.

### 3.2 Companion accepts the digest

Extend the context accepted by `/api/strand/interact` (and the shared `LearningContext` type in the BlazeBPF agent):

```ts
context: {
  currentExercise?: ...; currentChallenge?: ...;           // existing
  observation?: ObservationDigest;                          // NEW: current run
  previousObservation?: ObservationDigest;                  // NEW: previous run, for before/after
  prediction?: string;                                      // NEW: what the student predicted
  phase?: 'predict' | 'compare' | 'explain';                // NEW: set by the client or derived on the server
}
```

Behaviour by phase (this is the Socratic part):
- **predict:** ask the student to predict a measurable thing (e.g. number of read calls) **before** showing numbers. Do not reveal the numbers.
- **compare:** show the relevant `facts`, ask what differs from the prediction and why.
- **explain:** only after the student engaged or is stuck. The model may explain, grounded in the digest.

### 3.3 Prompt rules for the companion (system prompt additions)

```text
You are a Socratic learning companion.
- Ask ONE question per turn. Do not give the final solution in the first turn.
- Escalate gradually: predict -> compare -> probe -> hint -> explain.
- You may cite numbers ONLY if they appear in the provided observation facts.
  Never invent, round up, or estimate figures. If you project an improvement, label it
  clearly as an estimate and state the assumption. Prefer asking the student to re-run
  and measure.
- Ground every question in the student's code and the observed facts.
- If observation data is missing, ask a clarifying question instead of guessing.
```

### 3.4 Explainer changes

`ai-explainer.ts` currently asks the model to "quantify potential improvements" (e.g. "reduce syscalls by 90%"). That invites numbers that were not measured.

- Replace that instruction with: cite only figures present in the data; label any projection as an estimate with its assumption.
- Make explanation **on demand** in Socratic mode: `POST /api/observe` should be able to skip the explainer (`explain: false`) so the student does not receive the full answer before being asked. Keep the current behaviour as the default for backward compatibility.
- Keep temperature low and the timeout/`null` fallback.

### 3.5 Number-grounding guardrail (new)

After the model replies, run a deterministic check: extract numeric tokens from the reply and verify each appears in `digest.facts` / digest fields, or is wrapped as an explicit estimate. If not, regenerate once with a corrective instruction, or fall back to a safe template question. Log violations. This is the guardrail that makes "the model never invents measurements" testable.

### 3.6 Before/after comparison

Persist the last digest per `userId + exerciseId` (reuse the existing observation persistence in DynamoDB). When a new run arrives for the same exercise, pass it as `observation` and the stored one as `previousObservation`. The comparison itself (deltas, percentages) is computed in code and added to `facts`, never by the model.

### 3.7 Profile signals (Phase 2)

Replace the stub profile in `strand-routes.ts` by reusing the real learning-profile and conversation-history services that BlazeBPF uses. Record per attempt: success, error type (compile failure vs flagged security vs resource limit), time spent, attempt count. Treat response time as a **noisy** signal (a slow answer can mean distraction); normalize per learner or use it only as a weak hint.

---

## 4. Requirements (acceptance criteria)

R1. Given an `ObservationResult`, `buildObservationDigest` returns a bounded `ObservationDigest` whose derived numbers are computed in code. 
R2. `/api/strand/interact` accepts `observation`, `previousObservation`, `prediction`, `phase` and remains backward compatible when they are absent. 
R3. In `predict` phase the reply contains a question and **no measured numbers**. 
R4. In `compare` phase every number in the reply appears in the digest facts (enforced by the grounding check, section 3.5). 
R5. On the demo scenario, the companion does not provide the final solution in its first reply. 
R6. A second run for the same exercise produces a before/after comparison with deltas computed in code. 
R7. If the model fails, times out, or the observation is missing, the system falls back to the previous behaviour (no crash, a polite message). 
R8. The explainer no longer instructs the model to invent quantified improvements, and can be skipped with `explain: false`. 
R9. Student code and observation data are handled with the existing auth, sandbox limits and content filter; nothing new is logged in clear that was not logged before.

---

## 5. Tasks

### Phase 1: demo slice (must ship)

1. Add `ObservationDigest` type and `buildObservationDigest` with unit tests (include edge cases: empty syscalls, resource limit exceeded, compile failure).
2. Add the number-grounding check as a small pure module with property tests (the repo already uses `*.property.test.ts`).
3. Decide the companion path for COBOL: either (a) port the Socratic behaviour (phases, hint ladder) into `strand-routes.ts`, or (b) move the BlazeBPF agent logic into `packages/shared-backend` so every vertical reuses it. For the demo, (a) is faster; document (b) as the target.
4. Extend `/api/strand/interact` with the new context fields and the prompt rules from section 3.3.
5. Add `explain: false` to the observe flow and update the explainer prompt (section 3.4).
6. Persist and retrieve the previous digest per user and exercise (section 3.6).
7. Minimal UI: a "Predict first" input before running, then show facts and the companion's question in the existing panel (`LearningStrandPanel`). Keep the existing observation viewer.
8. Add the demo scenario as an integration test using a recorded `ObservationResult` fixture (no live eBPF needed).

### Phase 2: personalization

9. Replace the profile stub with the real profile and conversation-history services; record attempt signals (section 3.7).
10. Align hint levels with `attemptCount` and learner profile in COBOL.
11. Centralize model configuration (the model id is read in two places with a hard-coded default; use one config).

### Phase 3: platform-wide

12. Make the digest consume `ObserveReport` so Fortran and eBPF verticals reuse it.
13. Deduplicate the observation files copied across `blazecobol` and `blazefortran` into `shared-backend`.
14. Add a lightweight evaluation: replay recorded sessions and measure how often the grounding check fails.

---

## 6. Observations and suggestions (from the review)

- The COBOL companion's profile endpoint returns hard-coded values; personalization claims depend on Phase 2.
- The explainer is one-pass and prompts for quantified improvements; fix as in 3.4.
- Two result shapes exist (`ObservationResult`, `ObserveReport`); pick one as the tutor's input and document it.
- Observation files appear duplicated between `blazecobol` and `blazefortran`; verify with a diff and consider moving them to `packages/shared-backend/src/observe`.
- The companion decides the response `type` by checking substrings in the query (`hint`, `explain`, `example`); this is fragile. Prefer an explicit `phase` / `intent` field.
- Model id has a hard-coded default in two places; centralize.
- Do not put source code or raw syscalls in logs beyond what is already logged.

---

## 7. Out of scope for the demo

Leaderboards, new verticals, fine-tuning, changing the eBPF probes, and any change to authentication or infrastructure.

## 8. Definition of done (demo)

The demo scenario in section 1 runs end to end on a recorded fixture and on a live run, the grounding check passes, and a short transcript (anonymized) can be exported for the conference slides.

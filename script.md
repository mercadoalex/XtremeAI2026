# SPEAKER SCRIPT: The Socratic Method in AI, a Tale from the Trenches

**Alejandro Mercado · XtremeAI 2026 · Tue Nov 24, 06:35 GMT (00:35 Mexico City)**
**20 min talk + 5 min Q&A · 19 slides · English**

> How to use this file on the second monitor: open it in a Markdown preview with a large font.
> Text in quote blocks (`>`) is what you SAY. Everything else is for you only.
> Slide numbers = the counter at the bottom right of the deck (`#/10` in the URL is slide 10).
> `[TODO]` markers are things only you can fill in. They are collected in section 5.
> Pace: about 2,150 spoken words, roughly 16.5 minutes at a calm pace (130 words per minute). Your war story on slide 15 (about 50 s) and the pauses, clicks and the audience-guess moment bring it to 19 to 20 minutes. These are estimates: do a timed run-through and adjust.

---

## 0 · ONE-PAGE CHEAT SHEET

### Timeline and checkpoints (talk ends at 20:00, Q&A runs 20:00 to 25:00)

| # | Slide | Start | Clicks | Length |
|---|---|---|---|---|
| 1 | Title | 0:00 | 0 | 0:30 |
| 2 | Alejandro Mercado | 0:30 | 0 | 0:35 |
| 3 | Glass house | 1:05 | 0 | 0:40 |
| 4 | The weight of distrust | 1:45 | **3** | 0:55 |
| 5 | Answers are cheap | 2:40 | **1** | 0:55 |
| 6 | The Socratic method | 3:35 | 0 | 1:20 |
| 7 | Why models guess | 4:55 | 0 | 1:20 |
| 8 | The project: Blaze Platform | 6:15 | 0 | 1:25 |
| 9 | Don't let the model guess | 7:40 | **4** (card on click 2) | 1:00 |
| 10 | Observe | 8:40 | 0 | 1:55 |
| 11 | Explain | 10:35 | 0 | 1:40 |
| 12 | Ask | 12:15 | 0 | 1:45 |
| 13 | One dialogue | 14:00 | 0 | 1:10 |
| 14 | What it looks like | 15:10 | 0 | 1:30 |
| 15 | From the trenches | 16:40 | **4** | 1:50 (with your story) |
| 16 | Pattern for Monday | 18:30 | 0 | 0:55 |
| 17 | Takeaways | 19:25 | 0 | 0:35 |
| 18 | References | 20:00 | 0 | skip (5 s) |
| 19 | Q&A | 20:00 | 0 | Q&A |

**Checkpoints (glance at the clock):**
- Slide 5 by **2:40**. Slide 8 by **6:15**. Slide 10 by **8:40**. Slide 13 by **14:00**. Slide 16 by **18:30**.
- If you are **more than 1 minute behind** at a checkpoint, use the cut list in section 3.
- If you are **more than 1 minute ahead**, use the "ahead of schedule" list in section 3 and slow down.

### The ten things to remember

1. **One sentence:** don't let the model guess what happened. *Observe it, let the model interpret it, let a tutor ask about it.*
2. **Hook:** glass house, then "Answers are cheap. Understanding isn't."
3. **Socratic question to keep:** *"How do you know that?"* Our answer: measurements.
4. **Three verbs:** observe, explain, ask.
5. **Three rules for the model:** only measured figures, estimates labeled, ask don't guess.
6. **The loop:** student code, observe, ask, explain, change code, observe again.
7. **Pattern for Monday:** predict, observe, compare, explain.
8. **Be honest about status:** *built* = observation, explainer, tutor with profile and hints. *Connecting now* = the tutor asking about what was measured.
9. **The numbers in the JSON and the dialogue are illustrative.** Say so.
10. **Never say** "eliminates hallucinations". Say "reduces" or "grounds".

### Delivery rules (from the organizer's tips + online talks)

- **Do not read the slides.** The slides are a visual aid. Speak to the camera.
- **Slow down.** Excitement speeds you up. Breathe between slides. A pause is cheap.
- **Value first.** The audience wants the takeaway, not the biography.
- **Pause 2 to 3 seconds** after: the glass-house question, "What if the AI asked instead of told?", and "How do you know that?".
- **Look at the camera**, not at the slides, when you say the key lines (they are in **bold** below).
- **Water nearby.** Phone on silent. Notifications off. Close unrelated apps.
- **If you stumble:** stop, breathe, repeat the sentence calmly. Nobody notices.

---

## 1 · SCRIPT, SLIDE BY SLIDE

### Slide 1 · Title · 0:00 → 0:30

**Clicks:** none

> Hello everyone, and thank you for joining, wherever you are in the world.
> I'm Alejandro Mercado, and today's talk is called *The Socratic Method in AI: a tale from the trenches*.
> It's about what happens when you teach an AI to stop giving answers, and start asking questions.
> I'll share what we built, what we got wrong, and one pattern you can use on Monday.

**Tip:** smile, look at the camera, speak slowly. Do not rush into the next slide.

---

### Slide 2 · Alejandro Mercado · 0:30 → 1:05

**Clicks:** none

> Quick context. I'm a DevOps and automation platform engineer, with more than twenty-five years across IT.
> I'm an IBM Champion, an AWS Community Builder and a Kiro Ambassador.
> But the reason I'm here is simpler: I care about EdTech. About using technology to help people really learn, not just consume content.
> This talk is about a personal project, built on my own time. Everything I'll show comes from that work.

**Tip:** 30 seconds maximum. The audience cares about takeaways, not biography. Move on.

---

### Slide 3 · Glass house · 1:05 → 1:45

**Clicks:** none

> Let me start with a question.
> *(pause, look at the camera)*
> **Would you build a glass house during an 8.0 earthquake?**
> *(pause 3 seconds)*
> Of course not. You can't build something fragile on a ground that moves. The foundations have to be something you can trust.
> Keep that image in mind, because I think many of us are doing exactly that right now. We're building real systems on top of AI, and we're not always sure what's under our feet.

**Tip:** the pause is the slide. Do not fill the silence. Optionally, ask in the chat: *"What's the worst hallucination you've seen?"* but only if you are comfortable reading chat live.

---

### Slide 4 · The weight of distrust · 1:45 → 2:40

**Clicks:** 3 (one per bullet)

> And that is how it feels.
> **[click 1]** You know what it feels like to build on uncertain foundations. A model tells you about an API that doesn't exist. Or a flag that was never in the docs. And it says it with total confidence.
> **[click 2]** One hallucination can ruin a critical deployment.
> **[click 3]** And that fear is not a flaw. It's a logical response to a technology that often prioritizes **fluency over truth**. It sounds right, even when it's wrong.
> So the question is not "how do we stop being afraid". The question is: how do we build something we can trust?

**Tip:** speak to the builders in the audience. Slow down on "fluency over truth".

---

### Slide 5 · Answers are cheap. Understanding isn't. · 2:40 → 3:35

**Clicks:** 1

> So here's the thing.
> **Answers are cheap.** Any model gives you one in seconds.
> **Understanding isn't.** If I copy a correct answer, I feel like I've made progress, but I haven't learned anything. Teachers call this the illusion of competence.
> Think of the last time an AI gave you a perfectly correct answer, and you learned nothing from it. It happens to all of us.
> *(pause 3 seconds)*
> **[click]** So, what if the AI **asked instead of told**? What if, instead of handing me the answer, it asked me the right question, about something real?
> *(pause)*
> That question is the whole talk.

**Tip:** this is the turn of the talk, from distrust to the proposal. Pause after the question.

---

### Slide 6 · The Socratic method · 3:35 → 4:55

**Clicks:** none

> This idea is not new. Socrates did it more than two thousand years ago.
> He didn't lecture. He asked questions until the other person found the answer themselves. He even described himself as a midwife: he didn't give birth to ideas, he helped others do it.
> There are classic kinds of Socratic questions. *Clarify:* what do you mean by "scalable"? *Assumptions:* what are you assuming about the input? *Perspectives:* what would someone who disagrees say? *Implications:* if that's true, what follows?
> These questions don't hand you an answer. They make you look at your own thinking.
> But I want you to keep one of them: **Evidence.**
> **How do you know that?**
> *(pause)*
> Think about it: someone says "my program is slow". How do you know? Did you measure it, or is it a feeling?
> Hold on to that question. In this talk, we answer it with measurements, not opinions.

**Tip:** do not read all five. Name two or three quickly, then land on Evidence (it is highlighted on the slide).
**If short on time:** skip the list, say only: "Socrates taught by asking. One question matters for us: how do you know that?"

---

### Slide 7 · Why models guess, and why questions help · 4:55 → 6:15

**Clicks:** none

> So why do models guess?
> Because they predict the **next token**, not the truth. A model is very good at producing text that *looks* right. But looking right and being right are different things.
> Without an external check, there's no grounding. And models sound fluent even when they're uncertain, which is what makes them dangerous.
> Ask a model how many times a program reads a file, without ever running it, and you'll get a confident number. That number is a guess.
> What helps? Ground answers in evidence. Ask verification questions. Make the reasoning explicit.
> Research has been converging on this from several directions: Socratic prompting, multi-agent debate, chain-of-verification, self-refine. Different names, same pattern: instead of one-shot generation, a loop of generate, question, check, revise.
> Asking is a way of checking. Now let me show you what this looks like in a real learning platform.

**Tip:** **do not read the four chips**. They are proof that the idea is converging, not content.
**If short on time:** skip the research sentence; say only "the research points the same way".

---

### Slide 8 · The project: Blaze Platform · 6:15 → 7:40

**Clicks:** none

> The project is called **Blaze Platform**.
> It started as a platform to learn eBPF. A learner writes code in the browser, compiles it, runs it, and works through guided exercises and challenges.
> eBPF has a reputation for being hard, and its error messages can be hard to read. That makes it a great test for a tutor.
> A typical session: the learner picks a challenge, writes code, runs it, and the platform shows what happened. That is exactly the moment where a tutor can help, or get in the way.
> Then we generalized it. Today, Blaze is an AI-based learning platform that is **agnostic to the technology being taught**, with verticals for eBPF, COBOL and Fortran, and more to come.
> Each vertical brings its own probes, so observability is built in. And they all share one contract: **observation, learner profile, and tutor**.
> You can find it at blazebpf.com. Fair warning: it's a work in progress, **not production ready yet**.

**Tip:** one platform, many technologies. The point is that the pattern is not tied to one language.
**Careful:** only public names. No hosts, IPs, bucket names or internal paths.

---

### Slide 9 · Don't let the model guess what happened · 7:40 → 8:40

**Clicks:** 4. **The eBPF card enters on click 2.**

> Here's the whole idea in one sentence.
> **Don't let the model guess what happened.**
> **[click 1]** **Observe** it, with eBPF.
> **[click 2: card slides in]** For those who haven't met it: eBPF is a Linux kernel technology that lets you run sandboxed programs inside the kernel, without changing the kernel source code or loading kernel modules.
> In plain words: the kernel sees every request a program makes to the operating system, and eBPF lets us watch them.
> **[click 3]** Then let the model **interpret** it.
> **[click 4]** And let a tutor **ask** about it.
> Observe, interpret, ask. Everything else in this talk is one of those three verbs.

**Tip:** do not read the card word for word. One plain sentence is enough.
**Key line:** observe, interpret, ask.

---

### Slide 10 · Observe: facts from the kernel · 8:40 → 10:35

**Clicks:** none

> Step one: **Observe.**
> When a student runs code, eBPF probes attach to the running program. They capture four kinds of facts.
> **System calls:** every request the program makes to the operating system, like reading a file. **Performance:** where the time goes. **Memory:** what it allocates and frees. And **security:** whether it does something it shouldn't.
> Say a student writes a program that reads a file. The probes see each read: how many, how big, how long it took. They see how much memory the program holds, and whether it tried to touch something outside its sandbox.
> The program runs in a **sandbox**, with resource limits, because running a stranger's code is risky.
> The result is a typed object we call *ObservationResult*. On the right you can see its shape: did it compile, how long did it run, how many times each system call was made, the peak memory, and whether anything security-related was flagged.
> The values on the slide are illustrative. The point is the shape.
> These are **facts.** Not opinions. Not guesses. Measured.
> **The model never has to guess what the program did. The kernel already told us.**

**Tip:** point at the JSON only once, then come back to the camera.
**Say clearly:** "illustrative values". If you have a real, trimmed example by Nov 24, say "this is a real one".
`[TODO: if you have a real ObservationResult, replace the slide JSON and drop the "illustrative" label.]`

---

### Slide 11 · Explain: the model interprets, it doesn't guess · 10:35 → 12:15

**Clicks:** none

> Step two: **Explain.**
> The model receives the student's source code and a compact summary of the observed facts, not the raw trace, so it sees what matters. And it produces an explanation **grounded in those facts**.
> To keep it honest, it follows three rules.
> **One: only measured figures.** Every number has to come from the data.
> **Two: estimates are labeled.** If the model projects an improvement, it must say it's an estimate, and state the assumption.
> **Three: ask, don't guess.** If data is missing, it doesn't fill the gap. It asks the student.
> Why? Because if you ask a model to "quantify improvements", it will happily tell you "this cuts system calls by ninety percent". A number nobody measured. It might even be right, but you can't know.
> Compare two answers. The ungrounded one says: "reading in blocks will make this ninety percent faster". The grounded one says: "your program made twelve hundred read calls of one byte each; fewer, larger reads should reduce that, but the exact gain depends on your setup, so measure again." The second one is honest, and it is checkable.
> And if the model fails, the system falls back. It never invents.

**Tip:** say the three rules, do not read the card text.
**STATUS CHECK before the talk:** present the rules as *implemented* only if they are in the product by Nov 24. Otherwise say: **"these are the rules we are putting in."** (Brief: `socratic-observation-bridge.md`, section 3.4.)
**Say only if asked:** low temperature and bounded output.

---

### Slide 12 · Ask: the tutor adapts to you · 12:15 → 14:00

**Clicks:** none

> Step three: **Ask.** This is where it becomes a tutor, not a chatbot.
> The tutor reads **how you answer**: whether you're right, how long you take, and what kind of mistake you make.
> It keeps a learner profile: concepts you've mastered, concepts you struggle with, your success rate, even a signal for motivation, so it knows when to encourage.
> And it uses a **hint ladder**, up to five levels, from a gentle nudge to the full solution. Imagine the file-reading problem. *(illustrative)* Level one: what happens each time the program calls read? Level two: how many bytes does it ask for? Level three: what if it asked for more at once? Level four: try reading in blocks. Level five: here's the code.
> The learner climbs only as far as needed. If the tutor sees you stuck, it raises the hint. When you get it right, it celebrates, because motivation matters too. And it remembers the concept you keep missing.
> This is not "next, next, next" through a syllabus. The path adapts to you.
> One caution: **response time is a noisy signal.** A slow answer may mean you got distracted, not that it's hard. So it's a weak hint, not a verdict.

**Tip:** this is the personalization slide. Emphasize "how you answer".
**Careful:** the profile and hint ladder exist in the original tutor; the COBOL companion is a lighter version. Do not claim features that are not live.
**If short on time:** skip the five-level example, say only "from a nudge to the full solution".

---

### Slide 13 · One dialogue · 14:00 → 15:10

**Clicks:** none

> Now let's put it together. **One dialogue.**
> The student writes code. We observe it. The tutor asks about what was measured. The model explains, grounded in the facts.
> Then the student changes the code, and we observe again. **The loop closes.** And that second measurement is the proof: not "the AI says it's better", but "we measured it".
> Let me be honest about where we are. Observation, the explainer, and the tutor with profile and hints are **built**. What we are **connecting now** is the last piece: the tutor asking about what was measured.
> These pieces exist. We are wiring them into one dialogue. That is the direction: a tutor whose questions are anchored in facts, not in the model's imagination.
> Next, we want to measure whether learners really need fewer hints over time.

**Tip:** say the status with confidence, not as an apology.
**STATUS CHECK before the talk:** only claim what is true on Nov 24. If the bridge is demoed, say so. If not, keep "connecting now".

---

### Slide 14 · What it looks like · 15:10 → 16:40

**Clicks:** none

> Here's what it looks like.
> A student says: *"My program is slow when it reads the file."*
> A normal assistant says "read in blocks" and moves on. The student pastes the fix, and learns nothing.
> Our tutor asks: **"How many times do you think your program asks the kernel to read?"**
> The student guesses: *"A few dozen?"*
> Before I show you the number, make your own guess. Type it in the chat, or just think of it.
> *(pause 5 seconds)*
> Now the measured fact: **twelve hundred calls, one byte each.**
> And the tutor asks: **"What does that say about how the file is being read?"**
> *(pause)*
> The student discovers it themselves: it reads one byte at a time.
> Then the student changes the code, we measure again, and the tutor asks: **"What changed, and why?"**
> This is an illustrative example. The pattern is what matters: **predict, then measure, then ask.**

**Tip:** perform the dialogue with two voices (student, tutor). Make it feel like a real exchange. The "make your own guess" pause is the **predict step** of your own pattern: let the audience live it. If nobody types, just keep the 5 seconds of silence.
**Say clearly:** it is illustrative. `[TODO: replace with a real, anonymized transcript if you have one.]`

---

### Slide 15 · From the trenches · 16:40 → 18:30

**Clicks:** 4 (one per bullet)

> Now, the part you came for: the trenches. Four lessons.
> **[click 1]** First: asking a model to "quantify improvements" **invites invented numbers.** The fix is to cite only measured figures, and label everything else as an estimate.
> **[click 2]** Second: an explainer answers, a tutor asks. They have to be **one dialogue**. We built them separately, and bringing them together is the work we're doing now.
> **[click 3]** Third: response time is a **noisy signal.** We learned not to over-trust it: a slow answer can mean a distracted student, and a quick one can mean a lucky guess.
> **[click 4]** Fourth: running student code is a **security problem.** Our learners are strangers, and their code can do anything. Sandboxes and resource limits are not optional.

`[TODO: YOUR WAR STORY, 45 to 60 seconds (about 100 words). Structure: 1) what happened, 2) why it surprised you, 3) what you changed.]`
`[Example shape: "One day we noticed that ____. We thought ____. In reality ____. So we ____."]`

**Tip:** one concrete, true story is worth more than all four bullets. If you do not have a story, say honestly what you learned and keep it short.
**If short on time:** show only bullets 1 and 2, and shorten the story.

---

### Slide 16 · A pattern you can copy on Monday · 18:30 → 19:25

**Clicks:** none

> Here's what you can use on Monday.
> **Predict. Observe. Compare. Explain.**
> Ask the learner what they expect to happen. Measure what really happened. Ask what differs, and why. Only now, let the model explain, grounded in the measurement.
> It doesn't depend on eBPF, or on any particular model. With unit tests, for example: instead of saying "your function is wrong", ask "which of these three tests do you think fails?" Run them. Then ask why.
> Or with performance: ask which function the student thinks is the bottleneck, run a profiler, then ask why the result was a surprise.
> **Anything you can measure, you can teach this way.**

**Tip:** this is the takeaway slide. Slow down. Count the four steps on your fingers if it feels natural.

---

### Slide 17 · Takeaways · 19:25 → 20:00

**Clicks:** none

> Four takeaways.
> **Observe before you explain.**
> **Ask before you answer.**
> Numbers come from **measurements**, never from the model.
> And personalize from **how people answer**.
> One thing for this week: pick something you can measure, and ask your learners to predict it first.
> *(pause)*
> And remember Socrates' question: **how do you know that?** Now you can measure it.

**Tip:** land on time. This is your real ending. Do not add new content.

---

### Slide 18 · References · skip

**Clicks:** none

> *(Do not talk. Advance immediately, or say:)* "The references are on the slide. The deck will be shared."

**Tip:** you may skip this slide with one extra press of the arrow key.

---

### Slide 19 · Q&A · from 20:00

**Clicks:** none

> Thank you.
> I'll leave you with a question, in the spirit of this talk: **what question would you want your AI to ask you?**
> You can find me on LinkedIn and on Medium.
> I'm happy to take your questions.

**Tip:** keep this slide on screen for the whole Q&A. Take a breath. Drink some water.
**If the chat is quiet:** use a backup question from section 2 yourself ("A question I often get is...").

---

## 2 · Q&A PREP (honest answers)

**Rule:** if you do not know, say so, then give your best guess or your experience. *"I'm not sure. My educated guess is..."* Authenticity earns trust. Never invent numbers.

**Never say:** "eliminates hallucinations", "guarantees", "100%". **Say:** "reduces", "grounds", "makes it checkable".

### Likely questions

**1. How do you stop the tutor from giving away the answer?**
> The tutor works with a hint ladder, up to five levels, and it escalates gradually based on attempts. The solution is the last level, not the first.
`[TODO: add your leak check, if any: a second-pass check, tests, reviews.]`

**2. Does it really work? Do you have numbers?**
> Honest answer: `[TODO: your real data]`. If there is none: "We're early. We don't have a controlled comparison yet. What we'd measure is how often learners solve a challenge with fewer hints over time."

**3. Does the model still hallucinate?**
> It can. Observation grounds the facts, but the explanation still comes from a model. That's why we have rules: cite only measured numbers, label estimates, and ask when data is missing. The best check is to run the code again and measure.

**4. Which model do you use?**
> The one available through Amazon Bedrock. We haven't compared alternatives yet. The pattern doesn't depend on the model.

**5. Is it safe to run student code?**
> Student code runs in a sandbox with namespaces and resource limits for memory, CPU and disk, and the concurrency is bounded. `[TODO: add any extra details you are comfortable sharing.]`

**6. Why eBPF?**
> Because it lets us observe a running program from the kernel, without modifying the program or the kernel code. The facts don't come from the model.

**7. Isn't withholding answers frustrating?**
> It can be, which is why the tutor detects when someone is stuck and raises the hint level. The goal is not to hold back forever, but to let the learner do the thinking first.

**8. Does it work for experts as well as beginners?**
> The learner profile tracks what you've mastered, so the tutor can skip basics and adjust hint levels. `[TODO: add anything you have seen in practice.]`

**9. What about cost and latency?**
> `[TODO: your real experience.]` If none: "We haven't optimized that yet. The observation step is fast; the model call is the main variable."

**10. What about learner privacy?**
> `[TODO: what you store and what you don't.]` Safe fallback: "We keep a learning profile and conversation history per learner. We take care with access and logging. I'm happy to follow up on the details."

**11. Is it open source? When can I use it?**
> It's a work in progress and not production ready yet. `[TODO: your plans.]`

**12. Does it work beyond eBPF?**
> Yes, that's the idea. COBOL and Fortran verticals exist, and they share the same contract for observation, learner profile and tutor.

### Backup question for a quiet chat

> A question I often get is: "isn't this just a chatbot?" My answer: a chatbot answers; this tutor measures first, then asks.

---

## 3 · IF THINGS GO WRONG

### You are behind schedule: cut list (in this order)

1. Slide 6: skip the list of question types. Say only the Evidence line. (saves ~40 s)
2. Slide 12: skip the five-level example. (saves ~30 s)
3. Slide 7: skip the research sentence. (saves ~30 s)
4. Slide 15: show only bullets 1 and 2, shorten the story. (saves ~40 s)
5. Slide 14: skip the last exchange ("What changed, and why?"). (saves ~15 s)
6. Slide 18: skip entirely.

Never cut: slide 3 (hook), slide 9 (thesis), slide 13 (the loop), slide 16 (the pattern).

### You are ahead of schedule

- Slide 15: tell your war story with more detail.
- Slide 12: walk through the five levels slowly.
- Slide 10: dwell on one of the four kinds of facts with an example of your own.
- Pause longer after the key lines.

### Technical problems

- **Slides freeze or the browser misbehaves:** say *"Give me a second."* Switch to the PDF backup (keep it open in another window).
- **A slide shows a placeholder or odd text:** keep going. Say the line out loud. The audience follows you, not the slide.
- **Your connection drops:** rejoin the Zoom, tell the moderator, and resume from the last slide number. Keep this script open to find your place.
- **You lose your place:** look at the cheat sheet and the checkpoints. Say the thesis sentence: *"Observe it, let the model interpret it, let a tutor ask about it."*

### If you blank out

> *"Let me come back to the key idea: don't let the model guess what happened. Observe, interpret, ask."*

---

## 4 · BEFORE YOU START (5 minutes before)

- [ ] Zoom desktop app open, updated. Camera and microphone tested.
- [ ] Deck open in the browser, **full screen (F)**, on slide 1. Test the clicks once.
- [ ] This script open on the second monitor, large font.
- [ ] PDF backup of the deck open in another window.
- [ ] Notifications off, unrelated apps closed, phone on silent.
- [ ] Water nearby. A clock visible (or the timer in the deck's speaker view, `S`).
- [ ] Restart done earlier. Plugged in. Hotspot ready as a backup connection.
- [ ] **Status check:** is the Explain rule set implemented? Is the tutor connected to observation? Adjust the wording on slides 11 and 13 accordingly.
- [ ] **Confidentiality check:** no employer name, no internal hosts, no user data on screen.

## 5 · OPEN ITEMS (the TODOs, collected)

**To write or decide (needed for the talk):**
- [ ] `[TODO]` **Slide 15, war story** (45 to 60 s): what happened, why it surprised you, what you changed.
- [ ] `[TODO]` **Slide 10, real ObservationResult** (optional): a real, trimmed JSON to replace the illustrative one.
- [ ] `[TODO]` **Slide 14, real transcript** (optional): a real, anonymized exchange to replace the illustrative one.

**Q&A answers to fill (only if you want specifics):**
- [ ] `[TODO]` Q1 leak check · Q2 results and data · Q5 sandbox details · Q8 experts vs beginners · Q9 cost and latency · Q10 privacy · Q11 plans.

**Other open items:**
- [ ] Confirm the license/credit of the Socrates photo (slide 6).
- [ ] Fix the tagline on the Blaze logo ("…connects these dots"), slide 8.
- [ ] Time a full run-through with a stopwatch. Adjust using the cut list above.

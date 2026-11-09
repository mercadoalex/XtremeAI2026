# REHEARSAL SCRIPT: The Socratic Method in AI, a Tale from the Trenches

**Alejandro Mercado · XtremeAI 2026 · recorded version (one take) · English · target 19:30, hard cap 20:00**

How to read this file:
- Plain paragraphs are what **you say**. Do not memorize them word for word; learn the flow and the **bold** key lines.
- **[CLICK]** = press the arrow key once. **▶ AI CLIP n** = the recorded AI voice (you stay silent for its length plus one second).
- *(italics in parentheses)* = a stage direction, never spoken.
- Slide numbers are the counter at the bottom right of the deck. Times are the start of each slide.
- Rehearsal tip: read it aloud once with a stopwatch, then again looking only at the **bold** lines.

---

## The whole talk in 10 lines (if you lose your place)

1. Glass house: you can't build on ground you can't trust.
2. Answers are cheap. Understanding isn't. **What if the AI asked instead of told?**
3. Socrates asked. The question to keep: **How do you know that?**
4. Models predict the next token, not the truth. Ground them in evidence.
5. Blaze Platform: one contract, observation, learner profile, tutor.
6. **Don't let the model guess what happened. Observe, interpret, ask.**
7. Observe (eBPF facts), Explain (3 rules), Ask (adaptive tutor, hint ladder).
8. One dialogue: the loop closes with a second measurement.
9. Trenches: five lessons, the fifth is the frozen tutor.
10. Monday pattern: **Predict, Observe, Compare, Explain.**

---

## Slide 1 · Title · 0:00

Hello everyone, and thank you for joining, wherever you are in the world.
I'm Alejandro Mercado, and this is *The Socratic Method in AI: a tale from the trenches*.

**[CLICK]**
**▶ AI CLIP 1** (~9 s): "Hello. I am an artificial intelligence assistant, and today I will ask questions, offer comments, and accompany Alex during his talk."

*(smile)* Thank you, and welcome.

---

## Slide 2 · Alejandro Mercado · 0:30

Quick context. I'm a DevOps and automation platform engineer, with more than twenty-five years across IT.
I'm an IBM Champion, an AWS Community Builder and a Kiro Ambassador.
But the reason I'm here is simpler: I care about EdTech. About using technology to help people really learn, not just consume content.
This talk is about a personal project, built on my own time. Everything I'll show comes from that work.

*(turn to the AI, smile)* Could you tell them, in a nutshell, what this talk is about?

**[CLICK]**
**▶ AI CLIP 2** (~16 s): "Sure. In a nutshell, the talk is about how to enable an AI to teach by asking questions rather than providing answers, relying on facts rather than on the model's own assumptions: the system observes, the model interprets, and a tutor poses questions."

*(nod)* Thank you. And I'll share what we built, what we got wrong, and one pattern you can use on Monday.

---

## Slide 3 · Glass house · 1:05

Let me start with a question.
*(pause, look at the camera)*
**Would you build a glass house during an 8.0 earthquake?**

**[CLICK]** *(the words shake for about 3 seconds: stay silent, let it land)*

*(pause)*
Of course not. You can't build something fragile on ground that moves. The foundations have to be something you can trust.
Keep that image in mind, because I think many of us are doing exactly that right now. We're building real systems on top of AI, and we're not always sure what's under our feet.

---

## Slide 4 · The weight of distrust · 1:45 (6 clicks)

And that is how it feels.

**[CLICK 1]** *(bullet 1)*
You know what it feels like to build on uncertain foundations. A model tells you about an API that doesn't exist. Or a flag that was never in the docs. And it says it with total confidence.

**[CLICK 2]**
**▶ AI CLIP 3** (~6 s): "Do you know of any instance of an AI hallucination that has surprised you?"
*(pause 4 seconds, look at the camera, let the audience think of their own)*

**[CLICK 3]** *(bullet 2)*
One hallucination can ruin a critical deployment.

**[CLICK 4]** *(bullet 3)*
And that fear is not a flaw. It's a logical response to a technology that often prioritizes **fluency over truth**. It sounds right, even when it's wrong.

**[CLICK 5]**
**▶ AI CLIP 4** (~8 s): "Why is this dangerous? Because hallucinations that are fluent are harder to catch."

*(nod)* Exactly. It sounds right, even when it's wrong.
*(talk normally for a couple of seconds, then:)*

**[CLICK 6]** *(the badge "trips": rotating, color-shifting spirals. Stay silent and let the audience watch it for 4 to 5 seconds.)*

*(optional line, only if it feels natural:)* That is what a fluent hallucination feels like from the inside: everything swirling, and all of it sounds real.

So the question is not "how do we stop being afraid". The question is: how do we build something we can trust?

---

## Slide 5 · Answers are cheap. Understanding isn't. · 2:40

So here's the thing.
**Answers are cheap.** Any model gives you one in seconds.
**Understanding isn't.** If I copy a correct answer, I feel like I've made progress, but I haven't learned anything. Teachers call this the illusion of competence.
Think of the last time an AI gave you a perfectly correct answer, and you learned nothing from it. It happens to all of us.
*(pause 3 seconds)*

**[CLICK]**
So, what if the AI **asked instead of told**? What if, instead of handing me the answer, it asked me the right question, about something real?
*(pause)*
That question is the whole talk.

---

## Slide 6 · The Socratic method · 3:35

This idea is not new. Socrates did it more than two thousand years ago.
He didn't lecture. He asked questions until the other person found the answer themselves. He even described himself as a midwife: he didn't give birth to ideas, he helped others do it.
There are classic kinds of Socratic questions. *Clarify:* what do you mean by "scalable"? *Assumptions:* what are you assuming about the input? *Perspectives:* what would someone who disagrees say? *Implications:* if that's true, what follows?
These questions don't hand you an answer. They make you look at your own thinking.
But I want you to keep one of them: **Evidence.**

**[CLICK]**
**▶ AI CLIP 5** (~2 s): "How do you know that?"

*(smile, wait one second)* Good question.
Think about it: someone says "my program is slow". How do you know? Did you measure it, or is it a feeling?
Hold on to that question. In this talk, we answer it with measurements, not opinions.

*(Short on time: skip the list of question types. Say only: "Socrates taught by asking. One question matters for us:" and click.)*

---

## Slide 7 · Why models guess, and why questions help · 4:55

So why do models guess?
Because they predict the **next token**, not the truth. A model is very good at producing text that *looks* right. But looking right and being right are different things.
Without an external check, there's no grounding. And models sound fluent even when they're uncertain, which is what makes them dangerous.
Ask a model how many times a program reads a file, without ever running it, and you'll get a confident number. That number is a guess.
What helps? Ground answers in evidence. Ask verification questions. Make the reasoning explicit.
Research has been converging on this from several directions: Socratic prompting, multi-agent debate, chain-of-verification, self-refine. Different names, same pattern: instead of one-shot generation, a loop of generate, question, check, revise.
Asking is a way of checking. Now let me show you what this looks like in a real learning platform.

*(Do not read the four chips. Short on time: skip the research sentence and say "the research points the same way".)*

---

## Slide 8 · The project: Blaze Platform · 6:15

The project is called **Blaze Platform**.
It started as a platform to learn eBPF. A learner writes code in the browser, compiles it, runs it, and works through guided exercises and challenges.
eBPF has a reputation for being hard, and its error messages can be hard to read. That makes it a great test for a tutor.
A typical session: the learner picks a challenge, writes code, runs it, and the platform shows what happened. That is exactly the moment where a tutor can help, or get in the way.
Then we generalized it. Today, Blaze is an AI-based learning platform that is **agnostic to the technology being taught**, with verticals for eBPF, COBOL and Fortran, and more to come.
Each vertical brings its own probes, so observability is built in. And they all share one contract: **observation, learner profile, and tutor**.
You can find it at blazebpf.com. Fair warning: it's a work in progress, **not production ready yet**.

**[CLICK]** *(an "s" slides in from the right and completes "dots")* Yes. That was on purpose.

*(Only public names. No hosts, IPs, bucket names or internal paths.)*

---

## Slide 9 · Don't let the model guess what happened · 7:40 (4 clicks)

Here's the whole idea in one sentence.
**Don't let the model guess what happened.**

**[CLICK 1]** **Observe** it, with eBPF.

**[CLICK 2]** *(the eBPF card slides in)* For those who haven't met it: eBPF is a Linux kernel technology that lets you run sandboxed programs inside the kernel, without changing the kernel source code or loading kernel modules.
In plain words: the kernel sees every request a program makes to the operating system, and eBPF lets us watch them.

**[CLICK 3]** Then let the model **interpret** it.

**[CLICK 4]** *(a small bee, the eBPF mascot, flies in: wait about 3 seconds for it to land)* And let a tutor **ask** about it.

Observe, interpret, ask. Everything else in this talk is one of those three verbs.

---

## Slide 10 · Observe: facts from the kernel · 8:40

Step one: **Observe.**
When a student runs code, eBPF probes attach to the running program. They capture four kinds of facts.
**System calls:** every request the program makes to the operating system, like reading a file. **Performance:** where the time goes. **Memory:** what it allocates and frees. And **security:** whether it does something it shouldn't.
Say a student writes a program that reads a file. The probes see each read: how many, how big, how long it took. They see how much memory the program holds, and whether it tried to touch something outside its sandbox.
The program runs in a **sandbox**, with resource limits, because running a stranger's code is risky.
The result is a typed object we call *ObservationResult*. On the right you can see its shape: did it compile, how long did it run, how many times each system call was made, the peak memory, and whether anything security-related was flagged.
The values on the slide are illustrative. The point is the shape.
These are **facts.** Not opinions. Not guesses. Measured.
**The model never has to guess what the program did. The kernel already told us.**

*(Point at the JSON once, then come back to the camera.)*

---

## Slide 11 · Explain: the model interprets, it doesn't guess · 10:35

Step two: **Explain.**
The model receives the student's source code and a compact summary of the observed facts, not the raw trace, so it sees what matters. And it produces an explanation **grounded in those facts**.
To keep it honest, it follows three rules.
**One: only measured figures.** Every number has to come from the data.
**Two: estimates are labeled.** If the model projects an improvement, it must say it's an estimate, and state the assumption.
**Three: ask, don't guess.** If data is missing, it doesn't fill the gap. It asks the student.
Why? Because if you ask a model to "quantify improvements", it will happily tell you "this cuts system calls by ninety percent". A number nobody measured. It might even be right, but you can't know.
Compare two answers. The ungrounded one says: "reading in blocks will make this ninety percent faster". The grounded one says: "your program made twelve hundred read calls of one byte each; fewer, larger reads should reduce that, but the exact gain depends on your setup, so measure again." The second one is honest, and it is checkable.
And if the model fails, the system falls back. It never invents.

*(Status check: say the rules are implemented only if true. Otherwise: "these are the rules we are putting in.")*

---

## Slide 12 · Ask: the tutor adapts to you · 12:15

Step three: **Ask.** This is where it becomes a tutor, not a chatbot.
The tutor reads **how you answer**: whether you're right, how long you take, and what kind of mistake you make.
It keeps a learner profile: concepts you've mastered, concepts you struggle with, your success rate, even a signal for motivation, so it knows when to encourage.
And it uses a **hint ladder**, up to five levels, from a gentle nudge to the full solution. Imagine the file-reading problem. *(illustrative)* Level one: what happens each time the program calls read? Level two: how many bytes does it ask for? Level three: what if it asked for more at once? Level four: try reading in blocks. Level five: here's the code.
The learner climbs only as far as needed. If the tutor sees you stuck, it raises the hint. When you get it right, it celebrates, because motivation matters too. And it remembers the concept you keep missing.
This is not "next, next, next" through a syllabus. *(a thin line crosses out "next, next, next" about a second after the slide appears: say this line while it is drawn)* The path adapts to you.
One caution: **response time is a noisy signal.** A slow answer may mean you got distracted, not that it's hard. So it's a weak hint, not a verdict.

*(Short on time: skip the five levels, say only "from a nudge to the full solution".)*

---

## Slide 13 · One dialogue · 14:00

Now let's put it together. **One dialogue.**
The student writes code. We observe it. The tutor asks about what was measured. The model explains, grounded in the facts.
Then the student changes the code, and we observe again. **The loop closes.** And that second measurement is the proof: not "the AI says it's better", but "we measured it".
Let me be honest about where we are. Observation, the explainer, and the tutor with profile and hints are **built**. What we are **connecting now** is the last piece: the tutor asking about what was measured.
These pieces exist. We are wiring them into one dialogue. That is the direction: a tutor whose questions are anchored in facts, not in the model's imagination.
Next, we want to measure whether learners really need fewer hints over time.

---

## Slide 14 · What it looks like · 15:10 (6 clicks; you are the student, the AI is the tutor)

Here's what it looks like.

**[CLICK 1]** *(you, as the student)* "My program is slow when it reads the file."

A normal assistant says "read in blocks" and moves on. The student pastes the fix, and learns nothing. Our tutor does something different.

**[CLICK 2]**
**▶ AI CLIP 6** (~5 s): "How many times do you think your program asks the kernel to read?"

**[CLICK 3]** *(you, as the student)* "A few dozen?"
Before I show you the number, make your own guess. In your head, or in the chat.
*(pause 5 seconds)*

**[CLICK 4]**
Now the measured fact: **twelve hundred calls, one byte each.**

**[CLICK 5]**
**▶ AI CLIP 7** (~4 s): "What does that say about how the file is being read?"

*(pause)* The student discovers it themselves: it reads one byte at a time.
Then the student changes the code, we measure again, and the tutor asks:

**[CLICK 6]** *(optional)*
**▶ AI CLIP 8** (~2 s): "What changed, and why?"

This is an illustrative example. The pattern is what matters: **predict, then measure, then ask.**

---

## Slide 15 · From the trenches · 16:40 (5 clicks)

Now, the part you came for: the trenches. Five lessons.

**[CLICK 1]** First: asking a model to "quantify improvements" **invites invented numbers.** The fix is to cite only measured figures, and label everything else as an estimate.

**[CLICK 2]** Second: an explainer answers, a tutor asks. They have to be **one dialogue**. We built them separately, and bringing them together is the work we're doing now.

**[CLICK 3]** Third: response time is a **noisy signal.** We learned not to over-trust it: a slow answer can mean a distracted student, and a quick one can mean a lucky guess.

**[CLICK 4]** Fourth: running student code is a **security problem.** Our learners are strangers, and their code can do anything. Sandboxes and resource limits are not optional.

**[CLICK 5]** *(a small loading ring spins for a second and freezes: let it sit there while you tell the story)*
And the fifth one is the most humbling. **The boring failure.**
Our AI assistant had three quick-action buttons and a send box. It looked perfect in testing. But whenever the backend was slow or unreachable, it froze. All the buttons stayed disabled until you reloaded the page.
I was ready for hallucinations. This was not that. The request to the model had **no timeout**. The app just waited, and the "loading" state never switched off. And when the cloud returned an error page instead of JSON, the learner only saw "AI request failed".
*(Add in your own words HOW you found it and how it felt. Verify the final timeout value in the code.)*
We fixed it: give up after about thirty seconds, reset the state, and say clearly that the tutor is temporarily unavailable.
**A learner stuck in a frozen tutor learns nothing.** Failing gracefully is part of teaching.

*(Short on time: only lessons 1 and 2, and a shorter story.)*

---

## Slide 16 · A pattern you can copy on Monday · 18:30

Here's what you can use on Monday.
**Predict. Observe. Compare. Explain.**
Ask the learner what they expect to happen. Measure what really happened. Ask what differs, and why. Only now, let the model explain, grounded in the measurement.
It doesn't depend on eBPF, or on any particular model. With unit tests, for example: instead of saying "your function is wrong", ask "which of these three tests do you think fails?" Run them. Then ask why.
Or with performance: ask which function the student thinks is the bottleneck, run a profiler, then ask why the result was a surprise.
**Anything you can measure, you can teach this way.**

---

## Slide 17 · Takeaways · 19:25

Four takeaways.
**Observe before you explain.**
**Ask before you answer.**
Numbers come from **measurements**, never from the model.
And personalize from **how people answer**.
One thing for this week: pick something you can measure, and ask your learners to predict it first.
*(pause)*
And remember Socrates' question: **how do you know that?** Now you can measure it.

---

## Slide 18 · References · skip

*(Do not talk. Advance quickly, or say:)* The references are on the slide, and the deck will be shared. If you read one thing, read the guide on the right: the link at the bottom is a free excerpt from the publisher.

---

## Slide 19 · Q&A · 20:00 (2 clicks)

*(Stay silent about 4 seconds after you arrive.)*

**[CLICK 1]**
**▶ AI CLIP 9** (~3 s): "What question would you want your AI to ask you?"

*(smile)* Thank you. I'm happy to take your questions. You can find me on LinkedIn and on Medium.

**[CLICK 2]**
**▶ AI CLIP 10** (~4 s): "Thank you for listening. Keep asking good questions."

*(Stay silent about 5 more seconds so the video does not cut abruptly.)*

---

## If you are running long: cut list (in this order)

1. Slide 6: skip the list of question types.
2. Slide 12: skip the five-level example.
3. Slide 7: skip the research sentence.
4. Slide 15: only lessons 1 and 2, shorter story.
5. Slide 14: drop clip 8 and the last exchange.
6. Slide 18: skip.

Never cut: slides 3, 9, 13, 16 and AI clips 1, 3, 5, 6, 7, 9.

## Never say

"Eliminates hallucinations", "guarantees", "100%". Say **reduces**, **grounds**, **makes it checkable**.

## Checkpoints

Slide 5 by **2:40** · Slide 8 by **6:15** · Slide 10 by **8:40** · Slide 13 by **14:00** · Slide 16 by **18:30**.
Note: the slide 4 trip adds about 10 seconds. If you are more than 1 minute behind a checkpoint, use the cut list.

For Q&A answers and the references cheat sheet, see `script_voice.md` (sections 2 and 6).

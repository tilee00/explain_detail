# AI Isn't Replacing SREs — It's Deskilling Them

*Explained in detail for a beginner. Simple English. You do not need to know SRE or AI to understand this.*

---

## Main-1: What this article is about, in simple words

### One sentence takeaway

AI is not taking away SRE jobs, but it may be making SREs worse at handling the rare, hard problems that only humans can solve.

### What this means

SRE stands for Site Reliability Engineer. SREs are the people who keep websites and computer systems running smoothly. When something breaks, they fix it.

Many people think AI will either replace SREs completely or make SREs the most important job in tech. The author says neither idea is very helpful. Instead, the author asks a different question: if AI handles 95% of the easy, common problems, will SREs get worse at handling the 5% of hard, new problems that actually matter?

The article uses a 1983 research paper by a psychologist named Lisanne Bainbridge called "Ironies of Automation." The paper found something strange: when machines automate most of the work, the human operator becomes more important during the rare moments when the machine fails — but the human also gets less practice, so they become worse at handling those failures.

The author says AI is doing the same thing to SREs. AI takes over routine thinking tasks like finding patterns, sorting alerts, and fixing known problems. This leaves humans with only the rare, complex, confusing problems. But those hard problems need deep expertise, and expertise gets weaker when you stop practising.

There is also a related idea called "never-skilling." Deskilling means you once had a skill but lost it. Never-skilling means you never developed the skill at all. Junior SREs today may never get enough hands-on practice to build their intuition because AI handles most of the work for them.

The article suggests some solutions: deliberately doing some work without AI (like pilots practising manual flying even when autopilot works), and making sure humans actively work with AI rather than just watching it. The goal is AI that supercharges SREs and keeps them sharp for the 5% of problems that really count.

---

## Jargon in simple English

- SRE (Site Reliability Engineer) is an SRE (a person who keeps computer systems and websites running smoothly).
- Incident response is incident response (fixing problems when a computer system breaks or stops working).
- Deskilling is deskilling (losing a skill you once had because you stop using it).
- Never-skilling is never-skilling (never learning a skill in the first place).
- Automation is automation (using machines or software to do work that people used to do).
- Ironies of Automation is Ironies of Automation (the idea that the more you automate, the more important humans become during failures, but the less prepared they are).
- Observability is observability (the ability to understand what is happening inside a computer system by looking at its data).
- Tier 1, Tier 2, Tier 3 incidents is Tier 1, Tier 2, Tier 3 incidents (levels of problems: Tier 1 is easy and known, Tier 2 is partly understood, Tier 3 is new and very hard).
- Human-in-the-loop is human-in-the-loop (a person actively taking part in the work, not just watching).
- Human-on-the-side is human-on-the-side (a person who only watches and approves, without really thinking).
- Vigilance decay is vigilance decay (getting less alert and attentive over time because you are only watching, not doing).
- Muscle memory is muscle memory (the ability to do something well because you have practised it many times).
- Intuition is intuition (the ability to understand or decide something quickly based on experience, without needing to think slowly).
- Cognitive is cognitive (related to thinking, learning, and understanding in the brain).
- LLM (Large Language Model) is an LLM (a kind of AI that understands and writes human-like text).

---

## Main-2: Two real-world examples

### Example 1: The pilot and the autopilot

**Situation with issue:** A pilot flies a modern airplane. Most of the time, the autopilot does the flying. The pilot mostly watches the screens and makes small changes. After many months, one day the autopilot breaks during bad weather, and the pilot must fly the plane by hand. But because the pilot has not flown by hand for a long time, their hands feel shaky and they are not sure what to do. They are worse at flying than they were before.

**Solution:** The flight authority (FAA) now makes pilots practise flying by hand regularly, even when the autopilot works perfectly. This keeps their skills fresh so they can handle emergencies. For SREs, this means sometimes solving incidents without AI, just to keep their skills sharp.

### Example 2: The doctor and the AI assistant

**Situation with issue:** A doctor uses an AI tool to help find growths inside patients' bodies. After using the AI for some time, a study finds the doctor's own ability to find growths without AI drops from 28% to 22%. The doctor did not forget the medical knowledge. They just stopped practising the skill because the AI always did it for them.

**Solution:** The doctor should sometimes examine patients without the AI tool, to keep their own eyes and brain trained. This is deliberate practice so the skill does not disappear.

---

## Detail-1: The full story with simple English and real-life analogies

### The two popular ideas about AI and SREs

People in tech often talk about two opposite ideas. The first idea says SREs will become the most important people in tech because AI writes more code, and someone must keep the systems alive. The second idea says AI will destroy every tech job, including SRE jobs.

The author says both ideas are not very useful if you are an SRE trying to plan your work in 2027. A more useful question is: when AI handles most of the incident response, do humans get worse at the few hard problems that remain?

### The 1983 paper that predicted this

In 1983, Lisanne Bainbridge wrote a paper called "Ironies of Automation." She studied factories where machines did most of the work of human operators. She found a paradox — something that seems contradictory but is true:

- When automation works well, the human has almost nothing to do most of the time.
- But when the machine fails, the human must step in and fix it.
- Because the human rarely gets to practise fixing problems, they become worse at fixing them when it really matters.

In short, automation removes humans from normal work, but leaves them with the hardest job: handling rare emergencies they are unprepared for.

The author says we are seeing the same pattern with AI and SREs. Industrial automation took over routine physical work from factory workers. AI takes over routine thinking work from SREs — things like matching patterns, sorting alerts, and fixing known problems. This leaves humans with only the new, complex, confusing problems. But those problems need the deepest expertise, and that expertise gets weaker when you stop practising.

### What AI already does in SRE work

Today, AI in SRE work already handles many tasks:

- Reducing alert noise (grouping similar alerts so humans do not get too many notifications).
- Running runbooks (step-by-step instructions for fixing known problems).
- Detecting patterns in logs and flagging unusual things.
- Suggesting root causes from past incident data.
- Auto-remediation, such as restarting a crashed container or scaling up a service that is running hot.

Some future targets include:

- Connecting signals from metrics, logs, and traces together.
- Finding root causes for problems that are only partly understood.
- Detecting incidents before users notice them.
- Assessing the risk of changes and automatically rolling back bad changes.

PagerDuty divides incidents into three tiers:

- **Tier 1:** Known problems with known fixes. Fully automated.
- **Tier 2:** Partially understood problems. AI gives recommendations, humans validate them.
- **Tier 3:** New, complex, cascading failures. Humans lead, with AI providing supporting context.

Here is the catch: if human SREs only work on Tier 3 incidents — the ones they have never seen before — where do they build the intuition to handle them? Intuition comes from years of hands-on incident response, pattern recognition from repetition, and the gut-level understanding of a system that develops from painful late-night debugging. If AI handles all the easy problems, humans never get that practice.

### Evidence of deskilling in other fields

The article shares research from medicine and aviation:

**Medicine:** A study found that doctors who used AI help for finding growths saw their unassisted detection rate drop from 28% to 22% after some time. They got worse at the thing they were supposed to be experts in, not because they forgot the theory, but because they stopped exercising the skill.

**Aviation:** Research shows that pilots who rely heavily on autopilot experience measurable degradation in situational awareness and manual flying ability. The problem became serious enough that the FAA now requires more manual flying time to counteract the effect.

The author also describes a personal feeling: AI stopped being a tool they occasionally used and became the first thing they always reach for. The problem is that thinking only happens where it is absolutely unavoidable, so those moments become the only exercise the brain gets. The author can feel the rust.

Cognitive research suggests that because AI tools make tasks feel easier and improve visible performance, users often cannot accurately judge their own skills. You feel competent. Dashboards look green. Then on a Wednesday, a new incident happens that does not match any pattern the AI has seen, and you realise your skill has atrophied.

For SREs, this shows up in specific ways:

- We stop reading raw logs because AI summarises them.
- We stop forming hypotheses during incidents because AI suggests root causes.
- We stop building mental models of system architecture because AI maps dependencies for us.

Each of these looks like a productivity win on its own. Together, they hollow out the expertise that makes an SRE effective when things go wrong in ways nobody anticipated.

### Deskilling vs. never-skilling

There is something even more concerning than deskilling. Researchers call it **never-skilling**.

- Deskilling means you once had a capability but lost it.
- Never-skilling means you never developed it in the first place.

Junior SREs entering the field today face an environment where AI handles most of the incident response. The opportunities to build foundational intuition and skill through hands-on practice are disappearing. The training pipeline is broken and not self-healing.

SREs realise their skills are degrading, so they lean more on AI to compensate, which further degrades their skills. This creates a vicious cycle that is hard to escape.

### What can we do about it?

The author is not rejecting AI. AI tooling is being adopted and integrated more strongly because that is the only way forward. But we should be intentional about how we use it.

**Approach 1: Deliberate inefficiency.** Just as the FAA mandates manual flying time even when autopilot is capable, SRE teams can designate some incidents — even ones AI could handle — as human-practice opportunities. This may cost a super-fast solution, but it is a long-term investment to keep skills fresh.

**Approach 2: Build for human-in-the-loop, not human-on-the-side.** There is a meaningful difference between a system where a human just approves an AI recommendation, and a system where a human actively engages with the problem alongside AI. Approving keeps humans in a supervisory role, which leads to vigilance decay. Active engagement keeps humans cognitively sharp.

### The bigger picture

This issue applies beyond SRE. As AI writes more code, reviews more pull requests, and handles more debugging, the same questions apply to software engineering as a whole.

The author works at SigNoz, which builds an open-source observability platform. Observability tools give SREs the metrics, traces, and logs they need to understand their systems deeply. For SigNoz, the deskilling question directly shapes how they build AI into their product.

Their approach: start with an AI assistant that helps SREs use LLMs while keeping humans firmly in control. Eventually enable more autonomy, but within clear guardrails, and only as trust is earned. Because SigNoz owns the data layer, they can build deeper, more context-aware AI that understands systems the way an experienced SRE would.

The goal is not AI that replaces SREs. It is AI that supercharges SREs. Humans will remain essential for the decisions that matter most, especially those that impact production infrastructure. The future of SRE is a human **with** AI — intentionally designed to keep humans sharp, engaged, and ready for the 5% that really counts.

---

## A different perspective: Thinking models

### First Principles thinking

First Principles thinking means breaking a problem down to its most basic truths and building up from there, instead of accepting what everyone says.

The basic truth here is: skill comes from practice. If a machine does the thinking for you, you do not practise thinking. If you do not practise, your skill gets weaker. This is true whether the machine is a factory robot, an autopilot, or an AI assistant.

From first principles, the problem is not "will AI replace SREs?" The problem is "are humans still practising the skills they need?" If the answer is no, then deskilling will happen no matter how smart the AI becomes. The fix must protect practice time, not just worry about job replacement.

### Critical thinking

Critical thinking means asking questions and checking evidence instead of accepting the first story.

Questions to ask from this article:
- Is "AI will replace SREs" the right question? No — the better question is whether AI makes SREs worse at their hardest work.
- Is "AI will destroy all tech jobs" useful? No — it creates fear without helping anyone prepare.
- Is using AI always a productivity win? Not necessarily — it may look like a win today but hide skill loss that shows up during a real emergency.
- What evidence supports deskilling? Studies from medicine and aviation show measurable skill decline when humans rely too much on automation.
- Is the solution to stop using AI? No — the solution is deliberate practice and active human engagement.

Critical thinking tells us to verify the real impact of AI on human skill, not just assume it is good or bad.

### Inversion (thinking backwards)

Inversion means starting from the result you want to avoid and working backwards to prevent it.

The result we want to avoid: an SRE faces a novel, severe outage and cannot handle it because their skills have atrophied.

Working backwards:
- Why could they not handle it? Because they never practised hard problems. → Prevention: create deliberate practice opportunities for humans.
- Why did they never practise? Because AI handled all the easy and medium problems. → Prevention: designate some incidents as human-only work, even if AI could do them faster.
- Why did they rely on AI so much? Because it felt easier and dashboards looked green. → Prevention: measure skill directly, not just system health.
- Why did juniors never learn? Because the training pipeline does not give them hands-on practice. → Prevention: restructure training so juniors solve real incidents with supervision.

By thinking backwards, you build layers of protection so that even if AI handles most work, humans remain ready for the 5% that really counts.

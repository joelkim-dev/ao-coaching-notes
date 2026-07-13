---
date: 2026-07-08
tags:
  - conversation
  - coaching
---
_Conversation with [[Joel Kim|Joel]] and [[Joe Chrysler|Joe]] on July 8, 2026_

Our first coaching session, a few days into onboarding. Most of it was getting to know each other, with a solid block of practical coaching on how to learn fast, how to ask questions well, and how to build certainty that agent-assisted work is actually right.

# Top takeaways
- Ask questions until someone tells you to slow down. The more experienced person or team is the one who should decide a question is too much, not you. Go find that edge. Reaching it is fine and normal.
- Cache what you learn. When you ask a question, write it and the answer down, then reread your log each week. At a consultancy time is money, so absorb an answer once and keep it close.
- Prove your work by trying it as the end user would. Green tests are evidence, not proof. Open the review app, click around, and try to break it.
- Ask the delivery lead or team before you ask the customer. The answer may already be cached inside the team. Once nobody here knows, go to the customer.
- [[Why we like TDD|Test-driven development]] works no matter who writes the code, you or an agent. Start from an end-to-end test framed as instructions you would hand a human tester, then automate what you can.
- Pair widely and invest in relationships. Atomic is collaborative by default, not headphones-on solo work.

# What coaching is
- Weekly, an hour to start, and likely shorter as the Accelerator goes on.
- Think of it as access to an executive coach. Joe brings questions, Joel brings goals and topics. The aim is to keep Joel challenged but not overwhelmed, and to hand over tools whenever either edge shows up.
- Confidential to the two of them. It is an explicit place to process [[FUDA]] (fear, uncertainty, doubt, and anger) or anything else at work. Nothing needs to be polished here.
- Joe is a good triage point. If he lacks an answer, he can usually find who has it.
- Sessions are recorded, transcribed locally, and summarized into notes kept in a private coaching repo only Joe and Joel can read. See the earlier [[Pairing, Joel and Joe, 2026-07-07|pairing notes]] for how the work itself is going.

# Where the project stands
- Joel is still working the story to give a location an inactive status and hide it from the app, paired some with Nick. Playwright is running and an agent (Codex) is in the loop.
- Progress felt slow, but we covered important groundwork: navigating Git, running the review app, QAing a change, and [[rebasing]].

# Getting to know each other
This first session was mostly about learning how each of us works and where we come from.

# Learning at Atomic: five pillars
Joe's framework for picking up new skills quickly:
1. Do the thing on your project. The best way to learn a skill is to use it in context, which the project gives you constantly.
2. Build the mental model. Understand the shape of a thing, ideally how it works under the hood. Once you hold the model, like the definition of a triangle, you recognize the thing when you see it and can reason about how it behaves.
3. Externalize your brain with a good note system. Joe uses [[Obsidian]] because it works offline and is not tied to someone else's database.
4. Memorize the few things you use constantly. [[Spaced repetition]], for example an Anki-style deck of project terminology reviewed a few minutes a day, drives them deep enough that they stop costing you thought.
5. Learn by talking. Ask smart people about the things they know well.

Joel already leans on a doing-first, everything-is-a-skill view, nurture over nature, and asked to pair with a few different people beyond Nick early on to see how others work. Joe encouraged it.

# Asking questions well
- Ask until the team tells you to back off. The junior person is not the right one to decide a question is not worth the time. Teams will say when it is too much, and that is a fine edge to find.
- Being green is an asset. Sharing what confuses you helps the whole cohort, and it shows Joe what is worth writing down for the next new person. Much of why the [[TKM]] exists is exactly this: a cache of answers to questions that keep coming up. It is not the only place to look and not a replacement for all documentation.
- When you do ask, absorb and cache the answer so the next time it is in your memory or your notes.

# Building certainty your work is right
Joel's real question was how to QA work when an agent writes both the code and the tests.
- Frame it around the customer. You are hired to build software that works for the end customer, so the job is to build certainty it does, whoever or whatever writes the code.
- Know what the customer wants first. When unsure, ask, and ask the [[Delivery Lead|DL]] or team before the customer, since the answer may already be known here. The delivery lead stands in as a proxy for the client.
- Use [[TDD]] regardless of who types. Start from an end-to-end test: load the page, navigate, assert text, click, assert the new state. Write your [[Acceptance Criteria|acceptance criteria]] as instructions you would give a human tester, then turn as many as you can into automation. When the tests are green you have real evidence.
- Then check it yourself. Open the change, click around like a person, and try to break it. You can pass every test and still ship something that feels wrong to a user.

# Investing in relationships
- Atomic is social and collaborative. Most of your time should be working with someone, with solo stretches the exception rather than the rule.
- Pair lunches are a built-in way in: take another Atom to lunch on Atomic, submit the receipt to [[Ramp]], and don't repeat the same person within 30 days.
- There is more on offer: spin-downs, the office happy hour, Slack social channels you can join and leave freely, and Accelerator socials. The more you invest here, the better the experience.

> [!INFO] Factual Clarifications and Learning Opportunities
> These are things from the conversation that may be worth a second look. A language model wrote this section, so it is itself subject to fact checking.
> 1. Agent-written tests are not self-validating. When the same agent writes both the code and the tests, green tests mostly show the code matches the tests, not that either matches what the customer needs. Ground truth comes from acceptance criteria tied to the customer's intent and from trying the software yourself.
> 2. The term is "spaced repetition." Tools like Anki or the Obsidian spaced-repetition plugin schedule reviews at growing intervals so facts stick with less total effort.

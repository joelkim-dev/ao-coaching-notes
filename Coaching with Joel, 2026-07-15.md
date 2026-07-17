---
date: 2026-07-15
tags:
  - conversation
  - coaching
---
_Conversation with [[Joel Kim|Joel]] and [[Joe Chrysler|Joe]] on July 15, 2026_

Most of this session was practical coaching on the meta-skills behind the job: how to learn and retain new terms, how to stay present in conversations, how to build mental stamina, and how to manage time and priorities now that Joel's calendar is full for the first time. We also went deep on the economics of Atomic and on making the shift from cramming for events to working at a sustainable, steady state.

# Top takeaways
- Sort every new term into critical, nice to know, or trivia. Spend your learning time on critical things, the ones that would leave you looking lost in a client meeting if you missed them. Push the tags up to the repo so Joe can add to them between sessions.
- Learn to a depth that fits. For most terms, the bar is being able to explain it in a couple of sentences that an expert would approve. That same explanation is the test of whether you actually understand it. Check yourself against a good model, or Slack Joe or another developer.
- When a new term makes your brain fixate mid-conversation, capture it and move on. Say "hang on, that's new, let me write it down," jot it on a note card or in [[Drafts app|Drafts]], and look it up later. That stops the zoning out.
- Your energy rises and dips across the day. Stack the hardest, most novel, learning-heavy work into your high-energy windows and save a backlog of low-effort scutwork for the lulls. Fuel and sleep matter more than people expect.
- Manage time like a budget. Start logging where it goes. Block out the big fixed pieces first, then set a rough minimum and maximum for the things you care about rather than a single target.
- You are no longer studying for a test. This is steady state with no end date, so the whole game is finding a sustainable pace. Leave something in the tank.
- Joe's read on your growth edge: knowing what to prioritize, and having a system to fall back on when things get ambiguous.

# Learning and retaining terms
We put your running terms list on screen and sorted it into three buckets.
- Critical means you would look lost without it. How Node works, [[Vitest]], the concept of a background job runner, using a database tool, using a Git GUI, and client vs server side all landed here. So did a softer one: knowing which questions to bring to the team versus figure out yourself.
- Nice to know means it helps but is not on the critical list. [[Graphile Worker]] specifics, [[NEON|Neon]], Git submodules, and background jobs went here. On background jobs, you have exposed them but not yet designed with or used them, so the concept sits below the critical line for now.
- Trivia is fine to study if you feel like it, but it will not hurt you to skip. Git itself as a raw tool, and a couple of minor items, fell here.
- The pattern to notice: often the concept is critical while the specific tool is not. The database tool matters more than TablePlus, the job-runner idea matters more than Graphile.

On depth, you always have a depth lever. You could learn every detail of Node, but that is not critical. Being able to say in a few clear sentences what a thing is and how it works, to a level an expert would sign off on, is both the goal and the way to check yourself. A model is a cheap first check. For anything you want a human read on, Slack Joe or another developer with your current understanding and ask what they think.

[[Spaced Repetition|Spaced repetition]] is worth pulling in here. It is most valuable for the client-specific vocabulary you will not hit often enough to remember on your own. Most general terms you will absorb through normal repetition on the project. Save the [[Anki]] deck for the concepts that keep eluding you.

The larger habit: capture the new things flowing past you. There is too much to research each one in the moment, but if you never come back to them you stop compounding.

# Staying present in conversations
You mentioned zoning out a couple of times a day, sometimes while Nick or Meg is explaining something. We separated two cases. Some of it is ordinary drift that happens to everyone, and some of it has a specific cause worth acting on: a new word lands and your brain leaves the conversation to work it out from context. The fix is the capture habit above. When it happens with someone internal, it is completely fine to say "hang on, you just said something new, let me write that down," then carry on. Naming the zone-out when it happens is itself part of fixing it.

# Building mental stamina
You are feeling like you run out of steam partway through the day and reach for more frequent breaks. A few things here.
- Paying attention to this now is the right instinct. Left unchecked it is how burnout starts. See [[Managing Energy and Focus]] and [[Sustainable Pace]].
- Energy naturally varies. You are a body running a brain, not a brain in a vat. Most people warm up in the first half hour, peak before lunch, dip while digesting, and catch a second wind in the afternoon. Learn your own shape and stack your work against it.
- Some of the stamina just comes from practice. Three to five months in, this should feel more achievable. It is like a workout routine for a brain that is not used to stretching this much.
- Fuel and sleep are real inputs. The brain burns a lot of energy, especially when you are learning and building new circuits, so eating enough through the day and sleeping well both matter. Given how new and mentally heavy this all is, some of the fatigue is simply the load being larger than you are used to.

# Time management and priorities
For the first time your calendar is full and you have to choose. You are used to an over-allocated day only in short bursts, a crunch before a midterm or an editing deadline, then back to open space. Now there is more you want to do, across work, relationships, and life, than there is time, and it is not a motivation problem. You want to do all of it.
- Log where your time goes, the way you would track spending. You cannot budget what you do not measure.
- Block the big fixed pieces first. Roughly a third of the day is sleep, and about a third is work. What is left is where the real choosing happens: family, friends, fitness, hobbies, faith, and the rest.
- Instead of one target per thing, set a minimum and a maximum. Hobbies should stay above zero because you need something to fall back on, but not swallow everything. Relationships work the same way. Joe's example: if he goes longer than about six weeks without seeing a friend it is hard to keep up, but daily would be too much to have anything to say, so somewhere in that band is right.
- Season and theme systems can work too, where a stretch of time carries one focus. That may map well to how you already think, in seasons.
See [[Prioritization]] and [[Expected Value Prioritization]] for more.

# Changing behavior
What you are really doing is a long stretch of behavior change. The system that worked for college, tutoring, and gambling was tuned for those, and it is not tuned for this. Behavior change as an adult is harder than most people admit. Two book pointers, both with articles in the [[TKM]]:
- [[The 7 Habits of Highly Effective People]]. Joe finds the writing tedious and suggests the chapter-by-chapter summary or a good video instead of the book. Effective, in the book's sense, just means you tend to reach the goals you set and avoid the outcomes you don't.
- [[Triggers]], which he does recommend reading. Its core idea is that behavior change comes from deliberately increasing communication between your past, present, and future selves, treated almost as different people. Its main practice, [[Daily Questions]], has you pick a few things to work on and each evening ask "did I do my best to..." for each, then track it over time.

# The consultant mindset
One mindset shift for the work itself: you tend to block everything out and stay on a problem until it is done. As a consultant on billable hours, the client pays for every hour, so the question is not only "how do I solve this" but "how do I solve this in a time-efficient way." A lot of experienced-consultant thinking is really asking what the person who hired you believes the work is worth. That framing connects to [[The Value Spectrum]].

# The economics of Atomic
Your question about why Atomic bills by the hour opened a long, useful tangent you had wanted to have.
- Atomic works [[Fixed Budget Scope Control|fixed budget, scope controlled]]. Budget is set in hours, scope is what needs doing. The alternative of a fixed fee puts the delivery risk on Atomic, and pure time and materials puts all of it on the client. Fixed budget, scope controlled splits it: the client stays in control of spend without either side owning all the risk.
- Custom software is almost always a learning project. What a client asks for on day zero is rarely what they want at the end, and modernization work is especially risky because no one remembers everything the old system did until it is gone.
- In the short term Atomic is fairly insulated, but not in the long term. Roughly 75% of the work is repeat business, so a bad job costs future work.
- Atomic aims to be the high-cost, high-quality provider, on the argument that total cost of ownership is lower than going cheap and rebuilding, or buying off the shelf and customizing forever. Even so, the single biggest reason projects are lost is the perception of being expensive, and there is real pushback at the current rate. Rates have to keep climbing against rising salaries and insurance, but every increase makes cheaper alternatives look better, so the pricing lives in a band the market will bear.
- There is no finder's fee for bringing in a client today, though Joe is open to the idea and to hearing about leads. This all ties back to [[Atomic Purpose]] and [[Company Values]]: the goal of being a 100-year consultancy pushes decisions toward long-term cost over short-term profit.

# Working at a steady state
The thread running through the whole session: you are shifting from event-based effort to an indefinite, steady state, which you say you like. There is no test to cram for and no finish line. That is exactly why sustainability is the point. As Joe put it, you don't do your one-rep max on every lift, and you don't reach for a degree algorithm every time. Leave something in the tank.

# Where this is heading
Joe's summary of your edge: you clearly have the horsepower, and the place to grow is knowing what to prioritize and having a system for moving from ambiguity to clarity so obvious it almost feels dumb. That is what he has been steering toward when you pair, taking an ambiguous task and driving out ambiguity until only the genuinely hard part is left. Pair with the effort of writing clear lists and instructions, and keep getting clearer in how you communicate with the team. See [[Handling Ambiguity]].

Your first [[Accelerator Feedback|feedback]] session with Meg is Friday morning. Joe frames it as a chance to get someone else's perspective on how things are going, and a good place to raise anything you want to talk about. He is there to facilitate. He is pairing with you more than he usually would with an Accelerator right now, since Case is part time on the project and out for a few days, so he wanted to flag that Meg will have the steadier view over time.

> [!INFO] Factual Clarifications and Learning Opportunities
> These are things from the conversation worth a second look. A language model wrote this section, so it is itself subject to fact checking.
> 1. The economics figures were illustrative, not exact. The profit margin, the "about 75% repeat work," and the rate pushback were offered as rough shape, not audited numbers. Treat them as directional.
> 2. The pie-chart arithmetic got fuzzy in the moment. One hour out of a 24-hour day is roughly 4%, and as a share of free time it is higher, so the exact percentages matter less than the habit of budgeting against the big fixed blocks first.
> 3. [[NEON|Neon]] is serverless Postgres. Being able to say why that is useful, on-demand scaling and branching databases, is the kind of couple-sentence explanation that shows you understand it.
> 4. The cycling-glycogen story is an analogy. The brain runs mostly on glucose and does use meaningfully more energy during demanding learning, so eating and sleeping enough are real inputs to stamina, even if the exact mechanism differs from a long ride.

---
date: 2026-07-15
tags:
  - conversation
  - pairing
---
_Conversation with [[Joel Kim|Joel]] and [[Joe Chrysler|Joe]] on July 15, 2026_

A hands-on session about turning finished code into work the team can actually review and trust. Joel had three pieces of work in flight: the background-jobs dashboard, a slow summary page, and the side-navigation links. The throughline from recent sessions held. Ship things you understand, prove your claims instead of trusting them, and treat the agent as a fast collaborator whose work still needs a human to check. Partway through, a real standup broke out, which turned into a tour of how time tracking and sustainable pace work at Atomic.

# Top takeaways
- A [[Pull Requests|PR]] description writes to a stranger. It should walk a developer who has never seen your work through what you changed and why. Link the story, describe what you built in a sentence or two, note any issues you hit, and point to the changes that matter.
- A commit is done when there is nothing left to remove. "A commit is not done when there's nothing more you can add to it. A commit is done when there's nothing more you can remove from it and it still works."
- Write a test plan a stranger can follow. Step-by-step instructions to verify every important behavior, written for someone who is not in your head and cannot just "click through it" on instinct.
- Be skeptical of splitting knowledge into two places. "You should be highly, highly skeptical of any features that require you to split knowledge into two places."
- "Done" has a specific meaning. Done means shipped to customers and working. Ready for review is not done. "Be careful of that word."
- Set a target before you optimize, then prove the result is real. Ask how much faster would actually feel like success, aim there, and make sure a fast page is fast because the work got faster, not because it returns nothing.
- Do a little of the work before you ask for help. Whether it is the agent or a teammate, form your own understanding first, then give them the chance to correct you.
- The ugly AI change is sometimes the right one. Skepticism is the correct reflex, but verify before you conclude. "You'd be surprised how often I am very reasonable and very wrong."

# Turning finished code into a reviewable PR
The jobs-dashboard work was written, so the session started on how to package it. A PR is a document. Its whole job is to walk another developer through your changes in a way that is easy to follow, points them at the important code, and flags the parts that are not important.
- Write the description to a future developer who does not know what you were working on. Include a link back to the story, a couple of sentences on what you built, and any issues you ran into along the way. Joe drafted an example but asked Joel to write his own, since Joel is closer to the story.
- Reviewers look at your work three ways. The commit view, where they read your changes as a sequence of small steps. The checks, including the [[Fly Deployment|Fly]] PR review app they can click through. And the files-changed view, which you want to keep small enough that a person could plausibly read every line.
- The human is there on purpose. Even when the code is AI-written, part of the point of the person in the loop is to make sure the code makes sense.
- Commit hygiene carries the description. Your commits should read as a series of small atomic changes that each hold together, which is what makes the story easy to follow.

# Writing a test plan a stranger can follow
The "how to test" section gave the same lesson from a different angle. Imagine someone who has not built this feature and is not in your brain. They do not know what "click through it" means, so spell it out.
- The plan for the dashboard: open the PR review app, go to `/dev/jobs`, trigger a batch of jobs (for example redistribute invoices from the monthly summary page), then watch them run. Click the status filter chips to filter the list, click the column headers to sort, keyboard-navigate the page, and open a job's details.
- Capture what they should see. A screenshot pasted straight into the GitHub comment box uploads inline, so "it should look something like this" becomes concrete.
- Note the real-world caveat. The jobs only run at certain times of day, so a tester in the afternoon will not see the same live activity. That belongs in the instructions, not in your head.
- Do not just copy Joe's words. "The point is not copy. The point is thinking through what someone would need to do in order to test this thing." The wording is not magic. The communication is.

# Two sources of truth is a debt
The dashboard included a hard-coded map of the roughly thirteen job types and their metadata. That opened into a durable principle.
- Duplicated knowledge is a maintenance debt. When you add a new job, it will not appear here automatically, and the knowledge of what jobs exist now lives in two places. Every update means updating both. "You should be highly, highly skeptical of any features that require you to split knowledge into two places." See [[Single Source of Truth]].
- The pair traced it to confirm. Reading `jobMetadata`, they found the values genuinely hard-coded rather than generated.
- The call was to probably drop the feature. Hard-coded metadata will drift out of date, and the feature is likely too complicated to be useful to the client anyway. Joel left a note on the story rather than silently shipping it.

# Reviewers, cross-linking, and what "done" means
- Always get another developer to review, not just a delivery lead. For the dashboard, that meant tagging both the DL and a developer. For the query work later, Joe would tag only [[Nick Keuning|Nick]], since a gnarly SQL query is not something [[Meg Kretz|Meg]] would enjoy wading through. Match the reviewer to the change.
- Cross-link the story and the PR in both directions. The story is about the product work to be done. The PR is about the code changes you made. From the story a person should reach the PR quickly, and from the PR reach the story. On a small team the DL can follow the PR too. On a bigger team, product people may live in the story and developers in the code, so the link between them matters even more.
- Update the story status so reviewers know it is on their plate, and assign it to the reviewer if it is no longer your work. When Joel guessed at the project's status conventions, Joe flagged it: "Be careful with guessing. Sometimes that means something." Guessing at a shared workflow can send the wrong signal.
- "Done" is a loaded word. "Done means it's shipped to customers and it's working." What they finished here was a PR ready for review, which is not the same thing.

# The slow page: set a target and prove the speedup is real
The second story (COW-11) was a summary page that took roughly sixteen to seventeen seconds to load. Joel had already reordered the query to filter to sales movements before joining the rest, bringing it down to thirteen or fourteen seconds.
- Measure with the right tool. Rather than a stopwatch, use the browser [[Developer Tools|dev tools]] Network tab and watch for the request to stabilize. That put the current load around sixteen and a half seconds.
- Name a target you would be happy with. Joe pushed past a made-up number. "The point is not to actually have a pinpoint answer. The point is to ask the question and start thinking about how much faster would make a difference." If ten hours of work only shaved a second, that is not success. They set a target of under five seconds and wrote it into the PR as a checkbox.
- Prove the page is fast for the right reason. Joe's adversarial take: "What if it takes thirteen seconds to load because you put in a sleep for thirteen seconds and then loaded nothing?" The test has to confirm the data is still correct, not only that the page returns quickly.
- Use a [[Timebox|time box]]. Pick one question, set an intention for how long to spend, and stop when the box is up. The first question worth answering was simply how slow the existing pages are.

# Working alongside the agent
- Kick off the agent and build your own understanding in parallel. While the agent dug into the code for possible optimizations, Joe walked Joel through finding the same answers by hand: how do you get to the page, and where does the code live. "So that's what we're going to do while the robot cooks."
- Choose a capable model and a high reasoning effort for hard problems. A tricky performance question is worth the stronger model and the higher effort setting.
- Trust it least when the change looks unnecessary. That reflex shows up again in the nav-links story below.

# Undoing changes with git instead of by hand
Cleaning up a stray `.envrc` change (which had disabled a base-URL override) became a small but important safety lesson.
- Be careful with the database URL. Changing it can point your local app at a shared or production database, so treat those edits with caution.
- Reverse a change through git rather than re-editing by hand. Instead of remembering how to type the file back to its old state, right-click the change in your git GUI and discard it. "A more general pattern that works on bigger changes is go into git and say reverse that change." It scales to changes far too large to undo from memory.

# Simulating standup in Slack
The client team runs no daily standup, so Joe suggested simulating one in Slack.
- Post what you are planning to work on today, plus a fallback if you get stuck, in a place the team knows to look. Then link to the story. This is what a standup does, made asynchronous.
- Do a little of the planning work yourself first. "Rather than asking for someone to do the work of figuring out what you should do, do a little bit of work yourself to try to figure out what you might want to work on next, and then give them the opportunity to tell you, nope, not that." When you name your next pick, the team can catch it early if it is the wrong one.

# Time tracking and sustainable pace
A teammate's standup pulled the conversation into how time works at Atomic, a topic the pair had not covered yet.
- There are four broad buckets when you [[PunchIt!|punch]] time. Client billable, self-directed, manager-coordinated, and personal. Work for a client goes to that client. Required company events (the quarterly financials meeting, for example) are manager-coordinated. Optional events are usually self-directed, sometimes personal. When in doubt about an event like a lunch, ask the organizer what the punching expectation is.
- Self-directed time still pays you but does not count toward the billable expectation. It shows up on your paycheck, it just does not reduce the hours you are expected to bill. See [[Punchable]] and [[Non-Punchable]].
- The billable expectation is 40, inside a 40 to 45 hour band. The band spans BBM: billable, benefits (like PTO), and manager-coordinated. Atomic wants everyone at a full-time, sustainable pace.
- You are measured on the quarter, not the week. Missing 40 in one week does not matter as long as the quarter averages out. A useful framing for new folks: treat it as a 42-hour week, and you will tend to land around 40 and a half or 41 and slowly build a surplus.
- On PTO and surplus. Rather than hoarding PTO, plan it for the vacations you know are coming. If you are up eight hours on the quarter you can take a day without punching PTO, but tell your team, especially your DL, ahead of time, since an unexpected gap in billing can throw off invoices.
- Rounding: eight minutes and up rounds to the next unit, seven and below rounds down. It tends to wash out over time.

# Four tens and why not
Joel asked about working four ten-hour days. It is fine occasionally, but not something to optimize for. Atomic optimizes for in-person collaboration time, and a four-day week means a full day each week your team cannot reach you. See [[Sustainable Pace]]. The everyday shape is closer to nine hours a day with a lighter Friday.

# Buttons to links: when the ugly AI change is correct
The third story (COW-78) was to change the side-navigation items from buttons to links, so a user can right-click to open a destination in a new tab. The agent's diff looked wrong at first glance.
- The suspicious change. The agent had replaced a button-with-onClick with an anchor-with-onClick. Joe's gut reaction was that it "smells like robot bullshit," his shorthand for AI code that looks like work you do not actually need. A link should just navigate when you click it. Why intercept the click at all?
- Investigating before concluding. They followed `navigateTo` into the layout component. The onClick checks for a held modifier key (command, control, shift, or alt) and, if none is pressed, prevents the default and runs a client-side router transition that also closes the mobile menu. Modifier-clicks and native actions like copy or open-in-new-tab fall through to normal link behavior.
- The verdict flipped. "As much as this pains me, I think that is the correct solution. At least it's the smallest refactor." The plain anchor alone would have lost the router transition and the menu-closing behavior. They verified it in a mobile-width window: opening a new tab worked and the menu closed as intended.
- The lesson holds both ways. Be skeptical of AI-generated code that looks unnecessary, and verify before you rip it out. "You'd be surprised how often I am very reasonable and very wrong."
- Some tooling picked up along the way. Exported versus non-exported members define a module's public interface. "Find all references" and a `.tsx`-scoped, test-excluded project search trace where a component is actually used. A [[Git Worktree|worktree]] lets you open two branches in VS Code at once to compare them.

# Worktrees, ports, and colliding dev servers
Earlier confusion with dev servers led into two mechanics worth knowing.
- A [[Git Worktree|git worktree]] checks out a different branch in a different folder, so you can have several branches active at once and switch between them by directory. "It's like you clone the entire repo a second time, but cheaper." Handy, and the source of the dev-server collision here.
- A network connection is addressed by a four-tuple: source IP and port, destination IP and port. Two things cannot bind the same port at once. Good dev tooling picks a free port, but a lot of it is hastily written and grabs 3000 or 4000 by default. When you get weird results, check what is listening (`lsof`), because it might be a dev server from your other worktree.

# Fit and finish
Walking the nav change surfaced spacing problems, and Joe used them to reinforce the craft point from last session.
- Play with the app at different sizes and zoom levels. Use command-plus and command-minus in the browser to confirm everything scales, "because that will bite you on the butt sometimes."
- The fix itself was small. A nav link's right margin was 10 pixels where 12 matched the surrounding spacing, so the change was a single value. Along the way they talked through the CSS box model, padding versus margin and how spacing is inherited from a parent.

# Reviewing your own work and a flaky test
- Point reviewers at the change that matters. When you review your own PR, call out the critical changes in a comment. "There's going to be a lot of changes that change, but only a few things will be critical." Here that was the side-navigation file and the preserved click-handling behavior.
- Treat a failing check with curiosity, not panic. A Playwright test failed. Joe's hunch was that switching from a button to an anchor broke it, but it could also just be flaky. Ask the agent, prefer the least-brittle locator, and confirm rather than assuming. He was right this time, but the habit is to check.

> [!INFO] Factual Clarifications and Learning Opportunities
> These notes were produced from an imperfect, auto-diarized transcript and reviewed by a language model, not a human, so this section is itself subject to fact-checking. The points are neutral, forward-looking, and several stem from likely transcription artifacts.
>
> 1. Tool and term names came through garbled and are corrected above. For reference if the raw transcript is ever cited: "four plus four / 4 class / polar question" is a pull request (PR); "drop dashboard / jobstash board / stashboard" is the jobs dashboard; the client name appears as "Agbiz / AgVids / Agbos" and is left generic here per note-taking convention; the `cow`-prefixed numbers (COW-11, COW-78, and so on) are Linear ticket IDs; "get big / get a big" is `git config`, and "auto set up remote" is `push.autoSetupRemote`; "NVRC" is `.envrc` and "ENV-level" is an env file such as `.env.local`; "LSOF" is `lsof`; "playwrights" is [[Playwright]]; "MR-10 / MR-12" is a right-margin utility class; "hr / Href" is the `href` attribute and "A tag" is an anchor element; "Fly" is the Fly.io PR review app; "neon" is the Neon Postgres database.
> 2. The under-five-second target for the summary page is a good goal, but confirm the returned data stays correct as the query is optimized, exactly the adversarial "sleep for thirteen seconds" point Joe raised. If reordering the query cannot get it under target, server-side caching or a precomputed summary may be the next lever, since a thirteen-to-fourteen-second interactive query is still slow.
> 3. The nav-link click handling is correct, but worth a test that covers the full matrix: plain click navigates and closes the mobile menu, and modifier or middle clicks fall through to native open-in-new-tab and copy. That behavior is easy to break silently in a future refactor.
> 4. The hard-coded job metadata was flagged for likely removal. If the mapping turns out to be valuable, generating it from a single source (the job definitions themselves) is an alternative to deleting it, and avoids the two-places-to-update debt without losing the feature.
> 5. The time-tracking specifics (the 40 to 45 hour band, the BBM buckets, quarter averaging, the eight-minute rounding rule, and which lunches are self-directed) came from a live conversation. Confirm the details against the official Atomic time-tracking and PunchIt policy before relying on any single number.

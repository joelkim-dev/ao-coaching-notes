---
date: 2026-07-13
tags:
  - conversation
  - pairing
---
_Conversation with [[Joel Kim|Joel]] and [[Joe Chrysler|Joe]] on July 13, 2026_

A working session on the livestock client project. Joel is building a better dashboard for background jobs, and the code work kept opening onto bigger lessons. A [[Graphile Worker]] limitation, a confusing `dns` error, and a wall of unfamiliar terms all became teaching moments. The real center of the session was not the dashboard. It was how Joel manages the flood of new concepts an agent throws at him without either drowning in it or skating past it. Joe and Joel set up a learning backlog to hold that flood, and the throughline was the same one from the last pairing. Understand what you ship, and treat your own growth as a backlog you prioritize like any other.

# Top takeaways
- You are doing two jobs at once right now. You are producing work for the client, and you are producing capabilities in yourself. Both are required. Pick a rough split for the week, maybe start at 10% of your time on learning and grow it, and protect it.
- Stand behind your code. "The thing that separates us from vibe coders is we stand behind the code we write." A PR with your name on it is your professional stamp, whether you typed it or an agent did. "If you don't know what it's doing, you have no business writing your name on the code."
- Slow and understood beats fast and mysterious, especially early in your career. Nobody expects you to be a speed demon now. They expect you to know what you shipped.
- Capture new terms in a backlog and prioritize them. Joe set up a Kanban learning backlog in Joel's coaching notes so terms stop living as scattered browser tabs and start moving through levels of understanding.
- Managing that backlog is the same skill as consulting. When scope exceeds budget you increase the budget, cut the scope, or change what you build. Coaching is where you bring the backlog and get help telling transferable knowledge from trivia.
- Wading into ambiguity is the job, not a detour from it. Clients tell you the solutions they can imagine. Your work is to reverse those into the real problems and offer a better set of options.
- This is a production system. Don't guess. If the data you need to answer a question is not there, change the system so it captures it.
- Server code leaking into the client is easy to do in [[Next.js]] and worth learning to recognize. The `dns` error was a symptom of exactly that.

# The learning backlog
Joel named the thing that has been slowing him down. The agent uses terms he does not know, he has been stopping to research each one, and the list grows faster than he can clear it. "I feel like it slowed me down extremely significantly. It's kind of scary." Every term he learns spawns more terms.

Joe's answer was not to research less. It was to give the flood a home and a process.
- Build a backlog so the terms live somewhere connected, not as six open tabs you navigate by memory. It can be Obsidian, [[Drafts]], anywhere, but it has to be one place.
- Joe set one up as a Kanban board in Joel's coaching submodule, "Joel's learning backlog." The columns move a term through deepening understanding: a New terms inbox, then Level 1 the gist, Level 2, Level 3 build with it, Level 4 explain it to an expert, Level 5 explain it to a newcomer.
- Each card is just a link to the term's note. Drop the link in New terms when a word shows up, then make it a real note when you sit down to learn it. Leave evidence in the card that you reached each level. Drag cards to reprioritize.
- Capture fast, process later. Fire terms into Drafts the moment they appear so you never get overloaded, then turn through the backlog while you wait on the agent or whenever you have a spare minute.
- Store it in the coaching repo, commit and push it freely, and bookmark it since you will use it constantly.

The board is not the point on its own. It teaches the underlying move. "Prioritizing stuff like this, moving it along through a process, learning to leave yourself good notes" is what lets Joe walk in later and ask "what's on your learning backlog?" and get a real answer instead of a tour of scattered tabs.

# Learning something, not just seeing it
Joel can get a high-level sense of what code does from an LLM, and he knows that is not the same as understanding it. Joe's ways to close that gap:
- Build the concept in isolation. To learn something like an [[Error Boundary]], spin up a fresh empty project and build one with the LLM so you can see it on its own, away from the noise of the real app. Even when the LLM writes the code, read it by itself to see where it fits and what it is for.
- Push knowledge up, not just out. The LLM gives you a passable short-term understanding. Push technical concepts into your technical notes, and push the transferable, project-to-project ideas into the [[TKM|TKM]]. If all you ever get is the LLM's version, that is a shallow place to stop.
- Use people. Say "teach me this," then explain it back. Pull in a teammate or slack Joe with "here's what I think an error boundary is, is that right?"

# Scope, budget, and what to actually learn
Joel's backlog is huge and growing, and he worried he will never be done. Joe reframed it as the same problem we solve with clients every day.
- When scope exceeds budget you have three moves: increase the budget, cut the scope, or change what you build.
- Increasing the budget means spending more time. It works, but you hit diminishing returns, so grinding study sessions every night is not the real answer.
- Cutting scope means prioritizing. Look at the whole backlog and separate what matters from what does not. That is hard to judge alone, which is what coaching is for. "Bring me your backlog and I'll help you identify" what is worth deep study.
- Aim depth at transferable knowledge, the stuff that carries project to project or comes up in conversation with the team. Spend little on trivia like what one flag does or a single command's arguments.
- Pick a split you can hold for a week, something like 80% client work and 20% learning, and Joe suggested starting nearer 10% learning. You will absorb a lot just by doing the work.

# Consulting: living in the ambiguity
Joel asked how to get more specific direction. The client mostly said "just make it better," approved his feature ideas with "yeah, sounds good," and left him to figure out the rest. He also asked how to tell which questions to bring to the team and which to answer himself. Joe: "Welcome to the job. That is the whole job."
- You are not hired because the client knows what to do. You are hired because they do not. The consultant wades into the ambiguity, asks good questions, prototypes, and helps people figure out what they actually need.
- A useful model: the client has a set of problems and a smaller set of solutions they can imagine. They hand you a solution and say "I want this." Work backwards from their proposed solutions to the problems underneath, then judge whether what they asked for is really their best option.
- Your value is a wider set of options. You know the software and what it can do, so you can offer solutions the client could not picture. Joel pointed at how the senior members of the team, including [[Meg Kretz|Meg]], get past what a client says they want to what they actually need. That skill grows slowly, on small things first.

# The job dashboard
The client's current job view is weak. It shows only the 200 most recent jobs, you cannot page back, and you cannot see what happened or how long anything took. The team runs jobs and needs to see what is failing.
- Joel planned the work, had the agent break it into slices, and started on the second slice. The target is a live dashboard showing jobs in progress, queued, and failed, some sense of why a job failed, and a rough idea of how long jobs take, which the client specifically asked for.
- Two limitations surfaced from how [[Graphile Worker]] stores its data:
  - No job graph. Parent jobs fan out into child jobs, but Graphile does not record edges between them, so you cannot see which job triggered which. The outline view Joel wanted is not available for free.
  - No record of success. Graphile Worker removes a job once it completes successfully, so the data you have is only pending and failed jobs. "It doesn't leave any record around if you completed successfully."
- The fix Joe pointed at, for later: when a parent triggers a child, have the triggering function take an optional parent job id and store it in the database. And configure Graphile Worker to stop discarding completed jobs. Both are changes to a system we fully control.
- When Joel floated guessing at job relationships from available data, Joe drew a hard line. "This is a production system. Don't guess." And the broader point about custom software: we own the whole system soup to nuts. When you catch yourself saying "the tool doesn't give us X, so we can't," question it. Maybe we change the tool's inputs so it can.

# The `dns` error and the server/client boundary
The build broke with a dynamic `require` for the `dns` module, thrown from deep inside the Postgres driver (`pg`) in `node_modules`. Joe used it to teach several layers at once.
- Read the stack. Node is running the code, the failure is inside `pg`, and it is a dynamic `require` (not a static import) for `dns`. Something in Postgres wants the `dns` module and cannot find it.
- The tempting fix is the wrong one. They tried `pnpm install dns`, which does not address the cause. `dns` is a Node module, and Node does not run in the browser. So the true signal is that server-only code is running on the client, where Node's modules do not exist. "Something from the backend is running on the client and shouldn't be. Which probably means you're missing a boundary somewhere."
- [[Next.js]] makes this easy to trip over. When the frontend was one language and the backend another, you could not accidentally run server code on the client. Next lets you write both in JavaScript in one place, which is convenient and also makes it trivial to leak a server-only import into the client bundle.
- Getting unstuck from a broken build. You can chase imports one by one, or you can hand the whole picture to the agent that already has the context: "it looks like server code is leaking into the client, track it down." The agent proposed isolating the small error UI so it stops importing the server-connected dashboard module, and replacing database query types in the client tree with serializable UI types. Because the repo is set up to write a failing test first, the agent wrote a test that effectively checks the imports for you.
- `command shift F` searches the whole repo. It confirmed `dns` is not referenced anywhere in our code, which means something we depend on pulls it in, namely Postgres. The `dns` package we installed should come back out.

# Package managers and the monorepo
The `dns` error opened into fundamentals.
- A package manager references external dependencies for you, clones them, and drops them where the project needs them. In this language the OG is npm, with yarn, pnpm, and bun as alternatives. This project uses pnpm. Installing a dependency is `pnpm add <name>`, and you can target a specific workspace.
- `require` is JavaScript's older way to load a module. Joe sent Joel to [[MDN]] to read it and gave the short history: JavaScript began with no way to import files, then added `require`, then added `import`. That history will bite you on any JavaScript project sooner or later, so it is worth knowing.
- The repo is a monorepo, several independently buildable projects in one directory, each with its own `package.json`. Here that is web, the worker, database, and shared UI. A dependency has to be installed into whichever project actually needs it, so a bare `pnpm add` at the root can miss the mark.

# Git, GitHub, and submodules
Joel had learned the gist of a few git ideas over the weekend, and Joe filled them in.
- `git` and GitHub are not the same. Git is a program that runs entirely on your machine. GitHub is a place to store git repositories.
- Rebase, handled carefully. Bringing your branch up to date on its base before merging into develop is the idea. The danger is rebasing a branch other people have built on. Push a rewritten history and you can badly break their work. Rule for now: only rebase a branch that lives only on your machine, and if anyone else has based work on it, call Joe.
- Submodules need one extra step. The coaching notes live in a git submodule. Commit and push inside the submodule as often as you like, then update the pointer in the parent TKM and push that too, or the change is invisible to everyone else.
- Housekeeping along the way. They added a `.gitignore` for `.DS_Store` files, and renamed the learning-backlog file to drop a curly apostrophe, since fancy Unicode in filenames can trip up file systems. `rm -rf .next` was fine because `.next` is only a cache, but Joe flagged the general danger of `rm -rf`. It is recursive and forced, and it asks no questions.

# Writing good stories
Joel noticed many of the backlog stories are only a line or two, and asked what a good one needs. The short answer from Joe was yes, write more. A story should carry enough that a teammate can pick it up, which ties back to the description, [[Acceptance Criteria|acceptance criteria]], and estimate lessons from the last session.

# Tooling tips
- Three-finger swipe down on the trackpad shows every window of the current app, including minimized and hidden ones. Joe turned this on for Joel.
- Give terminal windows titles so you can tell Docker, the dev server, and the rest apart at a glance.
- A port 3000 conflict means something is already listening there, usually a stray `pnpm dev`. Kill it and try again.
- When you hit an internal server error, go find the logs where `pnpm dev` is running and keep working the problem with the agent.

> [!INFO] Factual Clarifications and Learning Opportunities
> These notes were produced from an imperfect, auto-diarized transcript and reviewed by a language model, not a human, so this section is itself subject to fact-checking. The points are neutral and forward-looking, and several stem from likely transcription artifacts.
>
> 1. Tool and term names came through garbled and are corrected above. For reference if the raw transcript is ever cited: "the robot" is the agent; "PMPM / PNTM / KMPM / PNDM / PMVM" is `pnpm`; "codex / codecs" is [[OpenAI Codex CLI|Codex]]; "L-Lens / L-up / L1" is the LLM; "chat chat chat be TV" is ChatGPT; "Mano Rebo" is monorepo; "Vytests" is Vitest; "graph file" is [[Graphile Worker|Graphile]]; "get up" is GitHub Desktop; "con-bond board" is a Kanban board; "post-cress" is Postgres / `pg`.
> 2. Installing a package named `dns` is not the same as Node's built-in `dns` module, and it does not fix the real problem. The `require('dns')` call comes from the Postgres driver (`pg`), which is server-only. The error appeared because that server code was being pulled into a browser context, where Node's core modules do not exist. The fix is to keep the server-only code out of the client bundle, and the added `dns` dependency should be removed once the boundary is fixed. Joe recognized this in the session.
> 3. Graphile Worker's retention behavior is worth confirming against its docs before building on it. By default it removes a job once it completes successfully while keeping failed and pending jobs, which is why only failures show up. Retaining a full history means recording your own job records or changing how completion is handled, so verify the exact configuration options first.
> 4. Graphile Worker has no built-in concept of a parent/child job graph. Storing an optional parent job id when a job is enqueued is a reasonable way to reconstruct the relationships. Just confirm how the parent's identifier is available at enqueue time so the link is reliable.
> 5. Bringing a branch up to date on its base (rebase or merge) is a different thing from a database data migration. Both came up close together in the conversation, so it is worth keeping the two ideas cleanly separate as they land in the learning backlog.
> 6. The learning-backlog ladder is captured above as five levels. The label for Level 2 came through the transcript unclearly, so treat the exact wording as provisional and settle it the first time you actually use the board.

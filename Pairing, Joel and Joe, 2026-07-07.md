---
date: 2026-07-07
tags:
  - conversation
  - pairing
---
_Conversation with [[Joel Kim|Joel]] and [[Joe Chrysler|Joe]] on July 7, 2026_

A long two-part session (morning and afternoon of the same day) early in Joel's onboarding onto a client project, a livestock operation that tracks its animals across many farm locations and recently moved off spreadsheets onto a custom app. Part one was orientation: access to the team's [[Linear]] backlog, a tour of how we work from a backlog, the shape of the repo, and a fight with the local dev environment. Part two moved into real work: designing and starting the first feature (letting a user deactivate a location and hide it from the app) driven by tests and built with an agent. The throughline was consistent. Understand the why before the how, keep your work visible and reversible, and don't let setup friction or an eager agent pull you off the goal.

# Top takeaways
- Understand why the business cares about the data before you touch the code. Joe opened with "have you gotten a sense of why we're tracking [this] yet? Why do we care?" Knowing what it costs the business when the data is wrong is what lets you make good calls later.
- Work from the backlog, and make every story carry its weight. A story needs a description anyone on the team can read, [[Acceptance Criteria|acceptance criteria]] that answer "how would we know this is done?", and an estimate. Estimate every story, even tiny ones, to calibrate your own bias, then compare actual time against the estimate to learn whether you skew optimist or pessimist.
- Build features as vertical slices. Database, API, and front end in one thin cut, not one whole layer at a time.
- Write the failing test first. See [[Why we like TDD]]. Test-first makes the feature testable by construction. Test-after risks building something you cannot test and then reworking it.
- Archive, don't delete. The mental model for this feature is archiving an email. Keep the record and its links in the database, hide it from the everyday UI, and let it come back.
- Don't gold-plate. Options are not free, they cost screen space and attention. A full audit-history table was more than this story needed. Build the smallest thing that satisfies the story and let the client tell you where to go next.
- Ship reversibly. Where you are unsure a change is wanted, build each surface as its own commit so you can revert or demo surface by surface. Unknowns are the client's call, and the delivery lead is a proxy for the client.
- Keep your work written down as you go: the design in a story doc, onboarding friction in a running list, and a handoff note before you step away.
- An agent is a force multiplier you still have to steer. It acts on your behalf on your machine, guarded by version control and your review. Read its diff, don't assume it is right.

# What we worked on
The feature: let a user deactivate a farm or location so it disappears from the app's main navigation while its data stays in the database and can be reactivated later. Two cases motivate it, both surfaced by [[Meghan Harris|Meg]] while she walked the domain:
- One-off locations. A place used a single time, like an overnight stop on a long haul, that then clutters the navigation forever.
- Locations no longer in active use whose historical data still matters. Old sources the team may still need to look up even though they are done using them day to day.

# Working from a backlog
Meg gave the domain context and set the priorities. The backlog lessons worth keeping:
- The backlog is a shared, ordered to-do list. Each item is a unit ("story", sometimes "user story" or "task") the whole team agrees on. Order eventually means priority, most important at top. Here the order is by value to the client.
- Verify you and your teammates see the same order. Linear's manual ordering is per-viewer unless you are in a shared view, so before trusting the priority, Joe and Joel confirmed they were looking at the same thing.
- Estimate everything. Prefer relative [[Story Points|story points]] or a working-time estimate. The main payoff early on is calibrating your own sense of size, so track actuals against your guess.
- Picking up a story starts with reading and understanding it. If it is missing a description, acceptance criteria, or an estimate, add them, then get team buy-in so your version matches what the team expects.
- Use the backlog as a scratchpad. If you spot something worth changing ("there are no sacred cows in this UI"), write it down and drop a story so it is not lost. Do not run far in one direction without buy-in.

# Getting the environment running
Most of part one and a chunk of part two was environment friction. The reusable parts:
- Read the README, and read it in markdown preview rather than raw. When you do not know a VS Code move, open the command palette.
- Where does anything in a repo live? The README. The app can seed dev data with `pnpm dev seed` once a required record exists, and it talks to an external sandbox API.
- Put the secrets file where it belongs. The app's secrets file (an `.envrc`) has to sit in the parent directory of the repo, not inside it and not in your home folder. They made a wrapper folder so it would not mix with other repos, moved the repo inside, and reopened the editor there. `direnv` and `asdf` manage the per-project environment, so a moved directory means re-running `direnv allow`.
- The repo is a monorepo: `apps/web` plus a background worker, `packages/database`, shared UI components, and shared pure TypeScript. Joe used the tour to teach the pieces: background job workers do slow work off the request path, [[Drizzle ORM]] maps [[Postgresql|Postgres]] rows and columns to objects and pointers, [[NEON|Neon]] is Postgres with copy-on-write branching, plus [[Turborepo]], [[Next.js]], Fly.io for hosting, and [[Pure Function|functional purity]] (a function with no side effects, easy to test because it depends only on its inputs).
- The Postgres port conflict. Migrations failed because a native Homebrew Postgres was already listening on port 5432 alongside the Docker one. The fix was to stop the stray Postgres. A small lesson landed here too: when the agent asserted a cause without checking, "don't assume that."
- The Playwright browser tests never ran. `turbo browser tests` failed because the [[Playwright]] browsers were not installed (the Chromium headless shell was missing) and `npx playwright install` did not resolve it cleanly. Joe chased a theory that Playwright had renamed its CLI, could not confirm it, and left a note to ask Nick and skip the browser tests for now. Joe also slacked [[Jared Currie|Jared]], who had worked on the project, to ask what was up. See the clarifications below, the rename theory is almost certainly wrong.
- Keep a running list of everything the README made you do that it did not tell you to do. That list is how the README gets fixed for the next person, and it is why Joe takes notes.

# Designing the feature
Before building, Joe walked the design in the open, in an Obsidian doc that became the story. The moves worth keeping:
- Vertical slice. A database column, a control on the detail page to toggle state, a column on the list page to show state, and a change to the nav so inactive locations hide. "I don't know if it's one component or more than one, we'll find out."
- From enum to timestamp. Joe first suggested an enum (active or inactive) over a boolean. On reflection they chose a nullable `deactivated_at` timestamp instead, because it records when the change happened, which matters when someone later asks why a location is gone. The rule the agent wrote: a location is active if and only if `deactivated_at` is null.
- Lightweight metadata over a full audit trail. They added an optional note and the acting user to a deactivation, which gives a cheap accountability story without a dedicated status-history table. They killed the full-history idea on purpose as over-building. On reactivation, clear the note, but never destroy data you might need. The app also already has an [[Audit Table|audit log]] to lean on.
- Archive, not delete, using the email metaphor above.
- Scope and reversibility. The main nav should fully hide inactive locations. The list page should default to hiding them but offer a reveal toggle, which is also how you reach the detail page to reactivate. For the other surfaces (movement pickers, other location pickers, search), whether inactive locations still belong is a business call, so build those as separate commits, demo them one at a time, and let the client decide what to keep.
- Options are not free. A toggle on every page is tempting, but it costs attention. Add one only where a user genuinely sometimes wants it, and let the business decide.

# Test-driven development
- Write a failing test first, then the feature. See [[Why we like TDD]].
- The reasoning Joe gave: test-first makes the code testable by construction, while test-after can leave you with something hard to test that you have to rework. It is a sensible default, not a rule for every situation.

# Agentic development
Part two doubled as a first real look at how we build with agents. What Joel saw:
- What an agent is. A harness ([[Claude Code]], or [[OpenAI Codex CLI|Codex]]) plus a model (here Opus 4.8). Instead of one prompt and one reply, the harness lets the model take many steps and act on your machine on your behalf. It is safe enough because you work under version control and watch what it changes, but you stay careful. More in [[Agentic Development]].
- Skills. The repo already had a `.claude/skills` folder. A skill is a markdown file, or a folder of them, that works like a reusable prompt telling the agent the repo's process ("read the testing policy before changing behavior", "don't skip tests or docs"). These arrived through Rhizome, an in-development internal tool from [[Drew Colthorp|Drew]]. Each project's skills are inspired by earlier projects rather than identical.
- A hybrid spec approach. Rather than pure spec-driven development, where you write the full spec and let the agent build it, they wrote part of the design into a `story.md`, worked with the agent to sharpen the open questions (should the note carry the acting user, should reactivation clear the note, which surfaces are in scope), and had the agent keep the doc updated as decisions landed.
- The prompt asked for pure functions wherever possible and for TDD, starting with a failing test.
- Reviewing the diff. They opened a git GUI and read what the agent did: a migration adding `deactivated_at` and `deactivation_notes`, a schema update, and tests that use an injected clock instead of the real one so time-dependent logic stays deterministic. That is the [[Dependency Injection|dependency-injection]] idea and the purity lesson from part one showing up in practice. "That seems legit."
- Handing off. Out of time, Joe had the agent write down its progress and remaining work, then committed on a feature branch with a WIP message that skips CI and pushed it, so Joel can check out the branch and keep going.

# Tooling and ergonomics
- Avoid full-screen, prefer maximize. Full-screen makes you watch space-switching animations all day when you flip between windows.
- Split the terminal in [[Ghostty]] (via its command palette) instead of letting one long-running command hog it.
- Set up a window manager (Rectangle, for `option`+`command`+arrow tiling) and a launcher with a clipboard manager ([[Raycast]]) in place of Spotlight.
- Use markdown preview to read docs, and the command palette whenever you do not know the shortcut.
- `pbpaste` drops clipboard contents straight into a file.
- "Whistle past the graveyard." When a migration re-ran oddly with no code change, Joe noted it and moved on, because all he needed right then was a running app.

# Smaller notes
- The note editor in the discussion tool is unreliable, so Joe composes elsewhere. He mentioned this is Vim, by the way ([[vim]]).
- While stuck on Playwright, Joe played the classic "Wat" lightning talk by Gary Bernhardt on JavaScript and Ruby oddities. Fun, and worth finishing later.
- Joel's closing line after watching the agent work: "agents are kind of OP."

> [!INFO] Factual Clarifications and Learning Opportunities
> These observations were generated by a language model reviewing an imperfect transcript, not a human, so they are themselves subject to fact-checking. They are neutral, forward-looking prompts, not criticism, and several stem from likely transcription artifacts.
>
> 1. Tool names came through garbled and are corrected above, flagged here in case the raw transcript is referenced later: "Durand / Duren / Durant" is `direnv` (`direnv allow`); "quad / quad code" is [[Claude Code]]; "codecs" is Codex; "chatubt / ChatTBT / Chachamaki" is ChatGPT; "PMPM" is `pnpm`; "pdpaste / pvpaste" is `pbpaste`; "Ghosty" is [[Ghostty]]; "comic object" is Atomic Object; "port 4.5.32" is port 5432; "a scale" is a skill; "TV pastor" is a TDD practitioner.
> 2. The theory that Playwright renamed its CLI is almost certainly incorrect. Playwright's test runner is still invoked through `npx playwright` and `@playwright/test`. The symptom, a missing Chromium headless shell, points to browsers not being installed, and the usual fix is `npx playwright install` (sometimes `--with-deps`). In a pnpm monorepo the binary may also need to run from the workspace that actually depends on `@playwright/test`. The lowercase "playwright" package that turned up is an unrelated tool, which is what made the rename idea look plausible. Worth confirming with Nick, but this reads as an install and workspace issue, not a rename.
> 3. The design moved from "use an enum, not a boolean" to a nullable `deactivated_at` timestamp. The timestamp is effectively a boolean plus a when, and it does not model more than two states. That is the right call for this story. If more statuses ever appear, an enum or a dedicated status table would come back into play.
> 4. "Neon can clone the whole database in constant time because copy-on-write" is a good approximation but optimistic. Branch creation is fast and cheap because data is shared copy-on-write, but it is not literally instant or free, and writes made after branching do consume storage.
> 5. "Write the test first and it is physically impossible to write a feature that can't be tested" is a useful overstatement. Test-first strongly pushes toward testable designs, but you can still write hard-to-test code. The guarantee is directional, not absolute.
> 6. On estimation, story points are a relative sizing unit, not a direct conversion to hours (see [[Story Points]]). Tracking your actual time against a points estimate is fine for personal calibration, but points and hours are different units, and treating them as interchangeable can mislead a team.

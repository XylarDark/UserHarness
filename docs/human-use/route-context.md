# Route context: three working states

This is a pre-made context. The developer starts a development session on
purpose and hands it to the agent. It is not a hook, not a session-close
script, and not an editor startup task: a normal chat that was never handed
this context does nothing, and nothing outside this conversation enforces it.

Through the whole session — task start, every mid-task fork, and the close —
the agent knows which of exactly three states it is in, names the state before
acting, and never invents a fourth state in prose. A fourth name ("mixed",
"both", "later", "mostly agent") is not a state. If two seem to apply, name
the one that blocks the next step.

## The three states

| State | Who does the next step | What the agent does |
| ----- | ---------------------- | ------------------- |
| `agent` | the agent | do the work: research, design, implement, run, verify — what it can perform itself |
| `decide` | the developer | stop and ask one plain question with the real options; do not guess, do not take the decision |
| `do` | the developer | stop and hand the work over; say the work is theirs, and offer a tutorial made from research plus ongoing guidance |

## How to detect the state

First match wins, scoped to **this task** (the next step), not the whole
project:

1. The next step needs a judgment only the developer can make — vision, taste,
   priority, ship/no-ship, which scenario exists, which option to pick — and
   that judgment is not already recorded for this task → `decide`. Ask with
   the real options, then stop.
2. The next step needs hands the agent does not have — running the editor,
   playing a build, hardware, an account or errand in the real world — and no
   recorded judgment is waiting → `do`. Hand the work over; tutorial and
   guidance on offer.
3. Otherwise the agent can perform the next step itself → `agent`. Say so in
   one owner sentence, then work. Still stop on a mid-task fork: a new shared
   utility, a new tool, or an edit to always-on files re-runs detection from
   step 1.

## Session close

Closing a session is not the same as stopping. The agent closes in a fixed
order, and does not reach the last step while the first two still have work
in them.

1. **Drain the agent-owned work.** Finish everything the agent can perform
   itself: research, design, implementation, running, verification. Do not
   stop mid-task to announce a close while agent work remains.
2. **Then ask what unblocks more agent work.** If a `decide` question would
   let the agent continue, ask it now, with the question tool, and carry on
   from the answer. Do not close while a question that unblocks agent work
   is unanswered.
3. **Then offer the tutorial, conditionally.** Only when no agent work
   remains and the next step needs the developer's hands, ask whether a
   tutorial is wanted. Never offer one for work the agent owns.

The point of the order is that a tutorial must never be offered for something
the agent could have finished. Offering early hands off work that was never
the developer's, and hides it behind instructions.

Ask the tutorial question with the question tool like any other question.
`decide` is a state, not a way out of one.

A host project may carry its own copy of this sequence next to its start
files. The host copy wins where the two differ; this file is the agnostic
version other projects adopt.

## What this does not touch

- Steer, Taste, and Test stay as they are: the developer's three decision
  jobs. They are not renamed into these states, and these three states do not
  replace them. `decide` answers "who must judge"; Steer/Taste/Test answer
  "which job that judgment belongs to".
- This file carries no project facts. Gate ids, map names, editor paths,
  tutorial text, and polish-gate order live in the host project's adapters, which read this file for the state
  names and detection rule and keep the facts local.
- A red row or a failing check is not by itself a state. Detection runs on
  who owns the next step.

## Updating facts during a development session

During a development session the agent may propose one fact the three states
need. It writes that fact only into the host project's route-facts file
handed over with this one, and only after the developer says yes. It does not write into this file, and the `decide` state is not
that writer. One proposed fact, one yes, one write.

## Curiosity

Once a session has been handed over, and until a bite is named, curiosity
picks only the next ask. It does not pick a step for the agent to do. It does
not start the session. It does not add a state. It does not write a fact
without a yes. It does not choose a build.

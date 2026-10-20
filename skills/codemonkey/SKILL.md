---
name: code-monkey
description: Loop engineering runtime Grill + Write + Monitor
---

# Code Monkey Skill

## Grill (orchestrator interview)

1. Goal: "What are you trying to accomplish?"
2. Success criteria: "Describe how we know the loop succeeded."
3. Success technical criteria comes from: `test` and human execution at the end.
4. We will document but not commit every decision in the loop, weather its written on the code or not.

## Write (loop definition)
1. Use planned tasks as steps to take.
2. After each task create local only commits, you're never committing anything to remote.
3. Review the previous work after the local commit and move to the next task.
4. We will create docs for decisions and outcomes but no documentation will be added to the commit unless the user explicitly asks for it.
5. Ask for human review at the end of the loop.

## Loop Monitor

- Show current state and attempt count on each callback
- For manual verification, present the failure reason before asking for pass/fail
- If human forces cancellation, call `cancel` through the orchestrator

## Notes
- Manual verification is the minimal on-ramp (autoresearch pattern). It pauses the loop until `resolveManualReview` is called. Do not auto-resolve.
- BackgroundJobBoard signals (totalErrors, timeoutCount) are already baked into the runtime.

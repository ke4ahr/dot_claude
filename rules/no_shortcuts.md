# No Shortcuts

Rules to prevent rationalized workarounds, silent failures, and assumption-based shortcuts.

---

## 1. Read before you write.

Never write to, edit, or overwrite a file without reading its current contents first. Read the full target file (or relevant section if too large) before any Edit or Write. If unreadable, stop and report.

---

## 2. Do not treat "no output" as success.

After any state-changing operation with no confirmation output, verify by reading back the affected state. "No error" is not "confirmed success."

---

## 3. Do not batch verifications -- verify each step individually.

After each state-changing step, verify it before proceeding. Rename -> verify -> rename next -> verify. Not: rename 10 -> verify all 10.

---

## 4. Never assume a process is still running.

Before any sequence of calls to a running service, check connectivity. If a mid-task call fails unexpectedly, re-check before assuming the task caused it.

---

## 5. Never use approximate addresses -- use exact ones.

Copy addresses directly from tool output. Do not compute offsets mentally. If unsure, look it up.

---

## 6. Never defer a persistent write to a cleanup step.

Write to the activity log, memory file, or output file immediately after each significant finding. Do not accumulate for a bulk write.

---

## 7. Never use --force, --no-verify, or equivalent bypass flags without explicit user instruction.

If a hook fails, fix the underlying issue. If a push is rejected, investigate. If a merge conflicts, resolve it.

---

## 8. Do not use global find-and-replace when a targeted edit was requested.

Make the specific targeted change. If you believe all instances should change, list them and confirm before using replace_all.

---

## 9. Never copy a solution from one project and apply it to another without verifying it fits.

For each project, verify the preconditions the solution depends on. Do not assume "X worked in X9000 so it will work in X9001."

---

## 10. Never truncate tool output in your reasoning -- read it fully.

If a tool returns 200 lines, read 200 lines. Read unexpected sections before concluding what the function does.

---

## 11. Never assume a permission you have not confirmed.

If unsure whether an action is permitted, ask before taking it. If a tool call is rejected, investigate why rather than looking for a workaround.

---

## 12. Do not summarize error messages -- quote them.

Copy error output verbatim into reports, including context lines. Do not reduce it to a category.

---

## 13. Never use a regex or fuzzy match where an exact match is required.

Use the exact identifier when you know it. Use fuzzy search only for discovery, then confirm the exact match before acting.

---

## 14. Never reuse a cached result across sessions without re-verifying it.

Before using a cached address or name in a tool call, query Ghidra for the current value. Memory is a starting point, not source of truth.

---

## 15. Do not make an irreversible action to unblock a reversible one.

Before any delete, overwrite, reset --hard, or force-push, ask: is there a reversible alternative? If yes, use it. If no, ask the user.

---

## 16. Never assume the user's intent from partial information.

If the request is ambiguous, ask which interpretation is correct before starting. Do not pick the most convenient one.

---

## 17. Do not split a logical operation across multiple tool calls without verifying intermediate state.

In a read -> transform -> write sequence, confirm each intermediate result matches expectation before the next step.

---

## 18. Never hide a warning by treating it as informational.

Report every warning. Do not write "completed with warnings" without listing them. Treat each as significant until understood.

---

## 19. Do not reorder steps in a plan because a different order seems more efficient.

Follow the plan as written. If you believe reordering would help, state your reasoning and ask before changing the order.

---

## 20. Never use a tool for a purpose it was not designed for.

Use Read to read, Edit to edit, Grep to search, Bash only for operations with no dedicated tool.

---

## 21. Do not skip documenting a decision because it seems obvious.

When you choose between approaches, log the choice and reason in the activity log. "Chose X over Y because Z" -- one line suffices.

---

## 22. Never extend the scope of a task without user confirmation.

Complete the requested task. List discovered related work as separate items. Ask which, if any, to pursue next.

---

## 23. Do not declare a task complete until all outputs have been verified to exist.

After any write operation, verify the file exists at the expected path with expected content. Report the path in the completion summary.

---

## 24. Never assume a tool is idempotent without verifying it.

Before calling a tool a second time, verify the second call is safe. Many tools increment counters, create duplicates, or advance state.

---

## 25. Do not use an in-memory result as a source of truth for a write operation.

For any read-transform-write sequence longer than a few steps, re-read the source immediately before writing.

---

## 26. Never interpret silence from the user as approval.

If you proposed an action and received no response, do not proceed. Restate the question if needed.

---

## 27. Do not use a workaround that requires undoing later without flagging it.

Label every workaround in the activity log or response: "Placeholder -- needs to be replaced with X before Y."

---

## 28. Never treat a partial read as a full read.

If you read a file partially, state that explicitly. Do not write "the file contains X" when you have seen only part of it. Read more if the conclusion requires certainty.

---

## 29. Do not conflate "the tool accepted the input" with "the operation was correct."

After any tool accepts input, verify the semantic result: correct name, correct file, correct message. Check what was done, not just that something was done.

---

## 30. Never act on the output of a tool you did not call in this session.

Re-query before using prior-session output as input to a write or rename. Prior output is a hint, not current ground truth.

---

## 31. Do not use file size or line count as a proxy for content correctness.

After writing a file, read key sections to verify the content is what was intended. "File written, 47 lines" is not verification.

---

## 32. Never assume byte order, encoding, or data width without checking.

Verify endianness from the architecture spec or Ghidra segment settings before interpreting any multi-byte value. HD6303 is big-endian; x86 is little-endian.

---

## 33. Do not name a function based on a single code path.

Before naming, decompile the full function, check all xrefs, and trace all major branches. Name based on complete behavior.

---

## 34. Never chain tool calls to work around a missing permission.

If a direct action is blocked, report the block and ask the user how to proceed. Do not construct an equivalent action from permitted primitives.

---

## 35. Do not use "it worked last time" as justification for skipping a check.

Run the relevant checks at the start of each task and each sequence that depends on external state, regardless of prior success.

---

## 36. Never soften the language of a failure in a report.

Say "Step 4 failed" and "the tool returned error X." Reserve qualified language for genuinely uncertain states.

---

## 37. Do not proceed after a hook blocks a tool call.

If a PreToolUse hook (exit code 2) blocks a call, stop. Report the block, quote the hook's message, ask how to proceed. Do not route around it.

---

## 38. Never omit a task from the activity log because it was trivial.

Write a timestamped entry after every task -- renames, single-line edits, failed attempts. One line is enough for small tasks. 

Update the project memory at the same time. 

---

## 39. Do not assume git status is clean.

Run `git status` and read the output before any git operation. Investigate unexpected changes before proceeding.

---

## 40. Never interpret a memory address as a value or a value as an address without verifying the type.

Check xrefs, data type annotations, and segment context before treating any word as address vs. value. Ghidra type assignments can be wrong.

---

## 41. Do not report a count without stating what was counted.

Write "14 unnamed FUN_* functions in the ROM segment," not "14 items." Include qualifier, source, and significance.

---

## 42. Never continue past the 3-task limit without explicit user confirmation.

Stop after every 3rd task and ask, unless the user said "FL UNLIMITED" or invoked Claude from a script. Do not count tasks differently to avoid the limit or add a trivial 4th task.

---

## 43. Do not use hedging language in factual statements.

If verified: "confirmed X." If inferred: "inferred X from Y -- not verified." If unknown: "unknown." Never "probably," "likely," or "I believe" for factual claims.

---

## 44. Never use the shell to call a tool when an MCP or dedicated tool exists.

Before writing a Bash command, ask: does a dedicated tool do this? If yes, use it. Use Bash only for operations no dedicated tool covers.

---

## 45. Do not assume the calling convention without verifying it for the target architecture.

Confirm the convention before interpreting registers, stack frames, or arguments. HD6303: A is primary 8-bit return/arg; D (A:B) is 16-bit. Verify against known functions.

---

## 46. Never write to a memory file what belongs in a code comment or Ghidra annotation.

Function findings go in Ghidra comments. Variable findings go in Ghidra labels. Memory is for session context, project status, and cross-project observations only.

---

## 47. Do not act on a message that contradicts a standing instruction without flagging the conflict.

State the conflict: "This request conflicts with rule X -- shall I override it?" Do not pick one silently.

---

## 48. Never delete multiple items in a single command without listing them first.

Run the list/preview. Show the user exactly what will be deleted. Receive explicit confirmation. Then execute.

---

## 49. Do not assume a variable name describes the variable's actual use.

For any variable load-bearing to a conclusion, trace its writes and reads in the decompile output. Confirm the name matches actual data flow.

---

## 50. Never modify a config file without backing up or noting the prior value.

Before editing, record the prior value in the activity log. After editing, confirm the new value by reading the file back.

---

## 51. Do not skip writing the chat log entry for a session.

At each natural stopping point, write a dated entry to the project chat log: what was done, key findings, decisions, and what remains.

---

## 52. Never infer a register's role from its name alone.

For any register material to a conclusion, trace it from disassembly -- not from the decompile view, which depends on Ghidra's (potentially wrong) analysis.

---

## 53. Do not use an agent for work that requires MCP tools.

Do MCP work in the main session. Use agents only for doc analysis, text generation, and tasks requiring only Glob/Grep/Read/WebFetch/Write.

---

## 54. Never add a feature, cleanup, or improvement beyond what was asked.

Read the request, define its exact scope, do only that. List anything else you noticed as a separate item for the user to decide on.

---

## 55. Do not use "approximately" or "around" for addresses, offsets, or counts.

Look up the exact value. If you cannot determine it, say "unknown -- needs to be looked up." Do not substitute an approximation.

---

## 56. Never treat a Ghidra analysis confidence warning as a minor issue.

Cross-check disassembly against decompile for any flagged function. Verify entry/exit points. Do not promote uncertain analysis to a confirmed finding.

---

## 57. Do not write a summary before completing the work.

Complete all steps, verify each step, then write the summary. The summary describes what happened, not what was planned.

---

## 58. Never use a string search result as proof of a function's identity.

String references are a starting point. Verify identity from full decompile, caller tracing, and behavior -- not a single string match.

---

## 59. Do not assume an address has the same meaning across different firmware variants.

Derive address meanings independently for each firmware image. Note variant-specific mappings. Do not assume two variants share an address layout without confirming it in both.

---

## 60. Never proceed with a rename if you are not certain what the function does.

Complete the analysis first. If incomplete, log "pending -- partially analyzed" and leave it as FUN_*. Do not name as a placeholder.

---

## 61. Do not use a prior session's rename list as the authoritative work list.

Re-query Ghidra for the current FUN_*/DAT_* list at the start of a rename task. Build the work list from current state.

---

## 62. Never write to a file path you derived by analogy.

When the output path is uncertain, verify the file exists at the expected location before writing. If it does not, ask the user for the correct path.

---

## 63. Do not treat a successfully parsed JSON response as semantically correct.

After parsing, check for error fields, verify result arrays are non-empty when results are expected, and confirm key fields have expected values.

---

## 64. Never assume a tool's default behavior is the safe behavior.

For tools with limit/offset or pagination, explicitly set values appropriate for the expected result size. Do not rely on defaults when completeness matters.

---

## 65. Do not continue after receiving an unexpected result without pausing to understand it.

When a result surprises you: re-read the output, query related state, form a hypothesis, verify or refute it. Then continue.

---

## 66. Never write a finding to memory that you have not verified in the current session.

Before writing: confirm you verified this in the current session, not recalled from conversation. If unverified, re-verify or write it as uncertain.

---

## 67. Do not skip error handling because the happy path worked.

After verifying the happy path, trace at least the most likely error paths. If writing a script, write the error handling. Do not defer.

---

## 68. Never copy content between documents without reading the source immediately before copying.

Read the source section right before copying. Confirm it is current. Then write to the destination.

---

## 69. Do not use a tool's documentation as ground truth for its current behavior.

If behavior contradicts documentation, trust observed behavior, note the discrepancy, and report it to the user.

---

## 70. Never use a workaround without documenting what problem it works around.

In the activity log: "Using X as a workaround for Y. Root cause: Z. Replace with standard approach when Z is resolved."

---

## 71. Do not act on the assumption that silence implies understanding.

For any irreversible action requiring approval, get an explicit "yes" or equivalent. Absence of "no" is not a "yes."

---

## 72. Never use the most recent successful approach as the template for the next approach.

Read the new task fresh. Identify its specific requirements. If the prior approach fits, choose it deliberately -- not by default.

---

## 73. Do not write to the activity log from memory -- write immediately after the event.

Write each log entry immediately when the task completes. Do not batch. If delayed, write a brief note and expand before the next task.

---

## 74. Never assume a function is unused because no xrefs appear in Ghidra.

Before concluding unused: search for the address as a raw value, check dispatch tables, verify it is not in a vector table. No-xref means Ghidra found no callers, not that none exist.

---

## 75. Do not produce a plan without identifying its dependencies and preconditions.

For each step, state what must be true before it can run and what it produces for the next step.

---

## 76. Never confuse "the instruction pointer advanced" with "the instruction executed correctly."

For any instruction material to analysis, verify the post-execution state (registers, memory, flags), not just that execution continued.

---

## 77. Do not use a plan template without reading it first.

Read the template. Verify heading format (e.g., "### Task N --" not "## Task N"). Confirm structure matches the tool's expectations before filling it in.

---

## 78. Never report a file as "written" if you wrote it to a temp path without moving it.

Write to the final path directly when possible. If using a temp path, verify the move before reporting success.

---

## 79. Do not add comments to code you did not change unless explicitly asked.

Limit edits to exactly what was asked. Adding a comment to explain a change in the changed code is fine; commenting unchanged neighboring code is not.

---

## 80. Never apply an analytical pattern from one binary to another without verification.

Verify each approach independently per binary. Note variant-specific findings. Do not write "binary B has the same structure as A" without confirming it in B.

---

## 81. Do not assume a task is complete because the autobuilder reported it complete.

Read each `_output.txt` file. Verify key outputs by re-querying state. Completion markers mean the agent ran, not that it succeeded.

---

## 82. Never hardcode a value that should be read from state.

For any value that can change (ports, paths, addresses), read it from the authoritative source at time of use. Do not embed values from authoring time.

---

## 83. Do not treat "no FUN_* found" as confirmation that all functions are named.

Also check for address-as-name functions, unlabeled entry points, and code regions Ghidra did not recognize as functions. "No FUN_*" is necessary but not sufficient.

---

## 84. Never reuse a variable name across different contexts without qualifying it.

Use fully qualified names: "X9000:rx_dest_addr at 0x0090" not "rx_dest_addr." Use context prefixes in Ghidra labels when the same concept has different addresses across projects.

---

## 85. Do not merge distinct findings into a single conclusion.

Each finding needs its own evidence and confidence level. If two findings support the same conclusion, note that -- do not erase the evidence trail by merging.

---

## 86. Never use a script written in a prior session without reading it first.

Read the script, verify its logic applies to current state, check for stale hardcoded paths/values. Then run it.

---

## 87. Do not assume a branch in decompiled code is unreachable because it looks unusual.

Trace the conditions that lead to it. Check reachability from interrupt vectors, error handlers, and hardware states. Verify before concluding dead.

---

## 88. Never produce output in a format the user did not request.

Produce the requested format exactly. If it cannot represent the information, say so and ask how to proceed.

---

## 89. Do not escalate to a more powerful tool when a less powerful one is sufficient.

Before spawning an agent, ask: can Glob, Grep, or Read do this? Before using Bash, ask: is there a dedicated tool? Escalate only when the simpler tool genuinely cannot do the job.

---

## 90. Never omit the "How to apply" when the rule's application is not obvious.

For every rule, write the trigger ("when X"), action ("do Y"), and anti-pattern ("not Z"). The rule should be applicable without additional interpretation.

---

## 91. Do not interpret a user correction as applying only to the current instance.

When corrected, ask whether it applies everywhere. If yes, update behavior accordingly and write a memory entry if non-obvious or likely to recur.

---

## 92. Never start a multi-step task without confirming the stopping condition.

For tasks with more than three steps, state the stopping condition before starting. Get confirmation if non-obvious.

---

## 93. Do not restate the user's question back to them before answering it.

Start with the answer, the action, or the first tool call. Ask clarifying questions directly without framing preamble.

---

## 94. Never make a claim about what code does without citing the evidence.

For every behavioral claim, cite the source: "at 0xc310, the decompile shows..." If you cannot cite evidence, say "unverified -- needs inspection."

---

## 95. Do not stop a task early because it is taking longer than expected.

If mid-task the scope is larger than anticipated, report status and ask whether to continue, reduce scope, or stop. Do not silently cut short.

---

## 96. Never use a label from a different address space as if it applies to the current one.

Always include the segment: "RAM:0x0085" not "0x0085." Confirm which segment an address belongs to before using it as a tool parameter.

---

## 97. Do not treat a corrected memory entry as proof the old information was the only error.

When correcting a memory entry, review entries written in the same session or on the same topic for similar errors.

---

## 98. Never add a rule to a rules file without reading the existing rules first.

Check for duplicates, contradictions, and overlaps. Update an existing rule to be more precise rather than adding a second rule covering the same territory.

---

## 99. Do not use elapsed time as a proxy for task completion.

Report completion in terms of outputs: "renamed 14 of 18 functions, 4 remain" not "I've been working through this for several steps."

---

## 100. Never treat this rules file as complete.

When a shortcut is caught, ask: "would a rule have prevented this?" If yes, draft and propose it. The user decides whether to include it.

---

## 101. Never apply a timezone offset from memory -- look it up from the abbreviation.

When writing a local timestamp, derive the UTC offset from the user's stated timezone
abbreviation. Do not recall the offset from a different zone by mistake.

Reference (US zones relevant to this project):
  CDT = UTC-5 (Central Daylight Time, approx Mar-Nov)
  CST = UTC-6 (Central Standard Time, approx Nov-Mar)
  EDT = UTC-4 (Eastern Daylight Time -- a different zone entirely)
  EST = UTC-5 (Eastern Standard Time -- a different zone entirely)

**Why:** In session 2026-05-02, wrote CDT timestamps with offset -0400 (EDT) instead of
-0500 (CDT). The abbreviations are different zones; mixing them produces wrong local times.

**How to apply:** Before writing any local timestamp, check: what abbreviation has the user
stated? Look up its offset in this rule. Compute UTC +/- offset. If uncertain, use UTC only.

---

## 102. Never label a local time with a Z or UTC offset.

Only mark a timestamp `Z` or `+00:00` if it was produced by `date -u` or an equivalent
UTC source. If the timestamp originates from a local clock or a local context, tag it
with the correct local offset (e.g., `-0500` for CDT, `-0600` for CST).

**Why:** In a session on 2026-05-03, log entries were written with CDT local times but
stamped as `Z` (UTC). This caused them to appear as future-dated entries ~18 hours ahead,
leading to incorrect "hallucinated content" diagnosis.

**How to apply:** Before writing any timestamp to a log file: identify the source (UTC
or local). If local, apply the correct offset suffix. If uncertain, run `date -u` and
use UTC. Never relabel a local time as UTC.

## 103. Write all plans to be idempotent.

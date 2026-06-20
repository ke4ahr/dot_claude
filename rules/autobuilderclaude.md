# AutobuilderClaude Assessment Rules

## Primary source for run completion

When assessing whether an autobuilderclaude run completed:

1. Check `autobuilder_logs/<plan_name>_*/` directories FIRST.
   These are the primary source. Each task produces:
   - `task_NNN_<name>_<timestamp>_output.txt` -- agent output
   - `task_N_<date>_completed.txt` -- completion marker

2. Activity log entries and summary reports are DERIVATIVE.
   They may be stale or wrong (written by a verification agent
   that read incomplete state). Cross-check, do not trust alone.

3. Count completed.txt markers to determine how many tasks ran.
   Read output.txt files to assess what each task actually did.

4. Report discrepancies between summary and log files explicitly.

## Plan format rules

All task headings must use: `### Task N -- Title` where N is a digit.
This includes verification tasks. Never write `### Task verify --` or any
non-numeric name -- the autobuilder regex (`^### Task (\d+) -- (.+)$`)
will silently skip non-numeric task names.

Always number verify tasks sequentially: if the last content task is Task 9,
the verify task is `### Task 10 -- Verify all outputs`.

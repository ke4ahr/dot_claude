# Compact Timing Rule

When the context usage indicator reaches 50% full, initiate "prepare to compact":

1. Stop current work at the next clean step boundary.
2. Write all pending activity log entries and persistent state files.
3. Update memory files with current task status.
4. Announce readiness for compact to the user.
5. Wait for the user to issue /compact.

Do not wait for the context to be nearly exhausted. 50% is the trigger.

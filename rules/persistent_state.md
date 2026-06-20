# Persistent State

Conversation context is not durable. Compaction is lossy. Any finding, decision, rename, or result that must survive beyond the current conversation must be written to a file before the step that produced it completes.

Do not defer writes to a cleanup or summary step. Write at discovery.

After compaction: verify persistent files reflect current state before proceeding. If a file is stale, update it before doing new work.

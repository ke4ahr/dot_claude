---
description: Post-compaction activities. Re-read all config and command files after a /compact operation.
license: SPDX-License-Identifier: GPL-3.0-or-later
---

Execute these steps in order after any /compact:

1. Re-read `~/.claude/settings.json`
2. Re-read `~/.claude/CLAUDE.md`
3. Re-read all `~/.claude/commands/*.md` files (one per Read call)
4. Re-read all `~/.claude/plugins/marketplaces/*/skills/*/SKILL.md` files (one per Read call)

All reads must complete before resuming any project work.
After all reads complete, resume where the previous session left off.

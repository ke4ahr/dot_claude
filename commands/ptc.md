---
description: Prepare To Compact
license: SPDX-License-Identifier: GPL-3.0-or-later
---
Prepare to compact. Execute these steps in order:

1. Stop current work at the next clean step boundary unless user has specified "cc" after ptc. e.g.: ptc cc
2. If the user has specified "ptc cc", stop immediately.
3. Write all pending activity log entries and persistent state files.
4. Update memory files with current task status.
5. Announce readiness: list what was done this session, what is pending, and confirm ready for /compact.
6. Wait for the user to issue /compact.

Do not continue working after announcing readiness.

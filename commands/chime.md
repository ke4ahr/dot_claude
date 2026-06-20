---
description: Play a chime. Use "attention" (needs input) or "completed" (job done) or another based on ~/.vocabulary/
license: SPDX-License-Identifier: GPL-3.0-or-later

---

Run `~/bin/playwave.sh <type>` where type is `attention` or `command completed` or other as specified or provided.

## Audio hook rules

All playwave.sh hook invocations (InstructionsLoaded, UserPromptSubmit, Notification, Stop,
StopFailure, PreCompact, ConfigChange) MUST run one at a time and finish before the next
begins. Never run two playwave.sh calls in parallel or overlap them.

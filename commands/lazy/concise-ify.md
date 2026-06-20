---
description: Suggest token-saving edits to any .md file that preserve all current meaning. Args: <file.md>. Show current vs. proposed side by side with a behavior-delta column.
license: SPDX-License-Identifier: GPL-3.0-or-later
---

1. Identify the target file from the prompt args. If no file argument is given, stop
   immediately and respond: "ERROR: no file specified. Usage: /concise-ify <file.md>"
   Read the target file.

2. For each line or logical rule, propose the shortest rewrite that preserves identical
   behavior as currently interpreted. Do not merge rules that have distinct triggers or
   scopes.

3. Present suggestions as a markdown table with columns:
   | # | Current | Proposed | Behavior delta |

   - "Current": exact text of the rule as written
   - "Proposed": shortest equivalent rewrite
   - "Behavior delta": "None" if behavior is strictly identical, or a brief note on any
     subtle change (e.g. "loses illustrative example, rule unchanged")

4. Verify each proposed rewrite:
   - Re-read the current rule and the proposed rewrite.
   - Confirm the proposed text triggers the same response in all cases the current text
     does, and no additional cases.
   - If a proposed rewrite would change behavior, mark it "BEHAVIOR CHANGE: <reason>"
     and do not include it in the final table.

5. After the table, report:
   - Estimated current token count
   - Estimated proposed token count
   - Estimated savings (tokens and percent)

6. Do not apply any changes. Present only.

---
description: Export the previous question, answer, reasoning, and tool calls as a dated markdown file to ~/claude/exports_general/.
license: SPDX-License-Identifier: GPL-3.0-or-later
---

1. Identify the previous exchange: the user message immediately before the current one,
   the assistant response to it, any reasoning/thinking present, and all tool calls made.

2. Derive the filename: take the question text, replace every "/" with "_", replace
   spaces with "_", strip punctuation except "_" and "-", lowercase, append ".md".

3. Write a markdown file to ~/claude/exports_general/<filename> with this structure:

   # <original question text verbatim>

   ---

   ## Question
   <original question text verbatim>

   ---

   ## Reasoning
   <thinking or reasoning steps, if any; omit section if none>

   ---

   ## Tool Calls
   For each tool call made while answering the question, one subsection:

   ### <ToolName> -- <brief description>
   Input:
   <tool input params, formatted as key: value or code block as appropriate>

   Result:
   <tool result or summary if very long; truncate with "... (truncated)" if over ~30 lines>

   Include: Bash commands, Read, Write, Edit, Grep, Glob, WebFetch, WebSearch,
            Agent invocations, Skill invocations, and any MCP tool calls.
   Omit section entirely if no tool calls were made.

   ---

   ## Answer
   <the assistant response text verbatim, excluding tool call output already captured above>

   ---

4. Run ren-date on the file:
   cd ~/claude/exports_general && ~/bin/ren-date.sh <filename>

5. Report the final renamed filename.

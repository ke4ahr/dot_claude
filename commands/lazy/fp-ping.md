---
description: Test FP and FP2 Ghidra bridge connectivity by connecting and making one dynamic tool call each. Use after MCP reconnect to confirm harness has re-announced tools. mcp_FP used by main VSCode instance. mcp__FP2 used exclusively by the autobuilderclaude. 
license: SPDX-License-Identifier: GPL-3.0-or-later
---

Execute these steps in order. Stop after step 7.

1. Call mcp__FP__list_instances. Set $FOO to the name of the first instance. Set $BAR to the name of the second instance. 

2. Call mcp__FP__connect_instance(project="$FOO").
   Report result (success or error verbatim).

3. Call mcp__FP__list_open_programs.
   Report result (success, or error verbatim).
   This is the dynamic tool test for FP bridge. If step 2 fails with a tool-not-found or
   input-validation error, FP harness has not re-announced -- dynamic tools unavailable.

4. Call mcp__FP2__connect_instance(project="$BAR").
   Report result (success or error verbatim).

5. Call mcp__FP2__list_open_programs.
   Report result (success, or error verbatim).
   This is the dynamic tool test for FP2 bridge. If step 4 fails with a tool-not-found or
   input-validation error, FP2 harness has not re-announced -- dynamic tools unavailable.

6. Report overall status:
   - FP bridge: UP / DOWN / PARTIAL (connect OK but dynamic tools unavailable)
   - FP2 bridge: UP / DOWN / PARTIAL

7. Stop. Do not proceed with project work until user confirms.

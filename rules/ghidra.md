# GMCP Tool Rules

## Hex Address Format

All address parameters in every mcp__FP__* call must be lowercase hex.
(e.g. `0xc310`, not `0xC310`)

## MCP Bridges Only -- No Direct REST Access

ALL Ghidra queries MUST go through mcp__FP__* tools. NEVER use curl or any
direct HTTP call to Ghidra REST backend ports. NEVER tell a subagent to query
Ghidra REST backends directly.

If an MCP tool call fails: STOP and report. Do NOT fall back to direct REST.
Do NOT discover backend ports and use them as a workaround.

Subagents and autobuilderclaude.py tasks DO have access to mcp__FP__* tools.
Ghidra work may be delegated to subagents and autobuilder tasks in this project.

## Workflow: connect_instance first

Before any Ghidra tool call, connect to the target project:
  mcp__FP__connect_instance(project="<project_name>")

This registers 174 dynamic tools as mcp__FP__<toolname> (no slot prefix).
Static tools always available without connect: list_instances, connect_instance,
list_tool_groups, check_tools, load_tool_group.

## vGMCP Virtual Slot Assignments

vGMCP slots are logical identifiers independent of physical ports.
Desired port = 8080 + slot number (vGMCP00 = port 8080, ..., vGMCP09 = port 8089).
All actual access via mcp__FP__* through FP bridge port 8060 ("bethington").

| vGMCP | connect_instance name      | Desired port | Description                                          |
|-------|---------------------------|--------------|------------------------------------------------------|
| 00    | X9000Retrospective        | 8080         | X9000 T53 radio drawer, VHF Low-Band                 |
| 01    | X9001Retrospective        | 8081         | X9001 HCN1033-T88 control head                       |
| 05    | X9AHRRetrospective        | 8085         | X9AHR radio drawer (VHF-Hi KE4AHR)                  |
| 06    | X9AHR_CH_Retrospective    | 8086         | X9AHR control head, HCN1045A                         |
| 07    | sb96sniff                 | 8087         | sb96sniff Windows binary                             |
| 08    | x9000dump                 | 8088         | x9000dump Windows PE x86                             |

Note: instances are user-started and user-stopped. The set running at any moment varies.
Before a session, verify which instances are up via mcp__FP__list_instances.
Last known running (s255 2026-06-12, user may have changed since):
  vGMCP05 X9AHRRetrospective            -- actual port 8088 (desired 8085; change on restart)
  vGMCP06 X9AHR_CH_Retrospective        -- actual port 8087 (desired 8086; change on restart)
  vGMCP01 X9001Retrospective            -- actual port 8086 (desired 8081; change on restart)
  vGMCP00 X9000Retrospective            -- actual port 8085 (desired 8080; change on restart)

## FP2 Bridge -- MRSS (DOS x86) Projects

A second Fastproxy bridge (FP2) serves RSS executable projects (x86 16-bit DOS MZ).
Access: mcp__FP2__connect_instance(project="<name>") then mcp__FP2__<toolname>.
These projects are NOT served through the FP bridge at port 8060.
All hex address rules, bridge unavailability rules, and connect_instance workflow
rules apply identically to FP2 -- substitute mcp__FP2__* for mcp__FP__* throughout.

## Connectivity Check

Do NOT pre-check the bridge before MCP calls. Make the MCP call directly.

All instances are served through the FP bridge at port 8060.

If an MCP call fails unexpectedly, THEN re-check once with:
`curl -s --max-time 2 http://localhost:8060/`

"Not Found" = bridge is up (the call failure was not a connectivity issue).
Connection refused = bridge is down -- follow Bridge Unavailability procedure below.

## Bridge Unavailability During Processing

If any mcp__FP__* call fails mid-task (connection refused, timeout,
or bridge error after a prior successful call):

1. STOP processing immediately. Do not attempt further GMCP calls.
2. Re-test the bridge port once (see ports above).
3. If still down: report to user with exact error, last successful operation,
   and what work remains. Do not guess at results or skip steps silently.
4. If back up: report the interruption and ask user whether to resume.

Do NOT continue processing and skip failed steps silently.

# Ghidra Notes

## Overview
Ghidra is a software reverse engineering (SRE) framework created by the National Security Agency (NSA). In our work with the Motorola Syntor X9000 firmware, we've been using Ghidra through MCP (Model Context Protocol) interfaces to analyze and rename functions, data items, and add comments.

## GhidraMCP Usage
All Ghidra instances are accessed via mcp__FP__* tools through Fastproxy (port 8060).
Connectivity check (on failure only): curl -s --max-time 2 http://localhost:8060/

Call mcp__FP__connect_instance(project="<name>") at session start.
After connect, use mcp__FP__<toolname> (no slot prefix, no GMCP number).

Project name to connect_instance mapping (vGMCP slots, s248+):
- "X9000Retrospective"      : vGMCP00 -- X9000 T53 radio drawer
- "X9001Retrospective"      : vGMCP01 -- X9001 HCN1033 control head
- "X9AHRRetrospective"      : vGMCP05 -- X9AHR radio drawer (VHF-Hi KE4AHR)
- "X9AHR_CH_Retrospective"  : vGMCP06 -- X9AHR control head, HCN1045A
- "sb96sniff"               : vGMCP07 -- sb96sniff Windows binary
- "x9000dump"               : vGMCP08 -- x9000dump Windows PE x86

FP2 bridge projects (mcp__FP2__connect_instance):

## Common Operations
- `get_function_by_address`: Retrieve function information at a specific memory address
- `get_current_address`: Get the currently selected address in the disassembly view
- `get_current_function`: Get the function at the current address
- `decompile_function`: Get the C-like pseudocode representation of a function
- `disassemble_function`: Get the assembly instructions for a function
- `rename_function_by_address`: Rename a function at a specific address
- `rename_data`: Rename a data label (variable) at a specific address
- `set_disassembly_comment`: Add a comment to the disassembly view at an address
- `set_decompiler_comment`: Add a comment to the decompiled pseudocode view at an address
- `list_functions`: List all functions in the program
- `list_data_items`: List all defined data labels and their values
- `get_xrefs_to`: Get all references to a specific address (who calls/references it)
- `get_xrefs_from`: Get all references from a specific address (what it calls/references)

## SB9600 Protocol Work
Our primary task has been renaming firmware elements to match the SB9600 (Serial Bus 9600) protocol documentation from Motorola. This involves:
1. Mapping opcode values (0x00-0xFF) to their SB9600 mnemonics (e.g., 0x20 = TXMODE, 0x21 = RXMODE)
2. Renaming functions that handle specific opcodes to SB9600_CMD_MNEMONIC format
3. Renaming dispatch table entries that point to opcode handlers
4. Adding descriptive comments to explain functionality
5. Naming data variables that hold protocol-relevant values (opcodes, addresses, state variables)

## Naming Conventions
Following the SB9600 rename plan, we use:
1. **Priority 1**: Exact SB9600 mnemonic or term from protocol docs (e.g., SB9600_CMD_TXMODE)
2. **Priority 2**: Descriptive behavioral name when no direct SB9600 term exists
3. **Subsystem prefixes** for X9001 variables (sci_, rx_, tx_, disp_, kbd_, etc.)
4. **Snake_case** for all names

## Verification
After renaming operations, we verify:
- No remaining FUN_* functions (unless intentionally left)
- Dispatch table entries properly named with SWD_ or SWD2_ prefixes
- Data items renamed from DAT_* to meaningful names
- Activity logs updated with timestamped summaries of changes

## Troubleshooting
- Bridge connection issues: Verify with `netstat -tlnp | grep 8060` rather than relying solely on curl
- MCP command failures: Some tools may not be available in certain contexts; alternatives include direct address operations
- Address formats: All addresses must be in lowercase hex (e.g., 0xc310, not 0xC310)
- Idempotency: Check current names before renaming to avoid unnecessary operations

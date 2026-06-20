# TokToken Codebase Index Guide

TokToken is a code exploration tool with a structured workflow for efficient codebase navigation.

## Core Workflow

**Initial Setup:** Run `toktoken codebase:detect` once per session. Cache the result—don't re-check.

**Before Queries:** If you've edited source files since the last index update, run `toktoken index:update` first.

**Standard Pattern:** Most questions require two calls:
1. Search for symbols with `search:symbols "<query>" --detail compact --limit 10 --exclude test`
2. Inspect top results using `inspect:symbol` with comma-separated IDs

## When to Use Specific Commands

- **inspect:bundle**: Only for import context or file structure needs (costs ~4x more tokens than inspect:symbol)
- **inspect:outline**: Understanding file contents and structure
- **inspect:blast**: Identifying what breaks from changes (Python/JS/TS/PHP only)
- **Other tools**: `inspect:cycles`, `inspect:hierarchy`, `find:dead`, `search:text`, `suggest`

## Key Efficiency Tips

Avoid these patterns:
- Multiple synonym searches (search once broadly instead)
- Multiple inspect calls when comma-separated IDs work in one call
- Using inspect:bundle for source code alone
- Using search:symbols when you already know the file location

Always use flags like `--detail compact`, `--compact`, `--exclude test|deps|vendor`, and `--limit N` to optimize token usage.

## Important Limitation

Import graph tools work well with Python, JavaScript, TypeScript, PHP, Ruby, Java, C#, Go, and Rust—but **not** with C/C++ codebases, which require `search:text` and `search:symbols` instead.

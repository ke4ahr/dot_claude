* Notifications: keep existing config unchanged.
* Tools: ask permission if previously configured to do so.
* Spelling: American English (synthesize, color).
* Dashes: use -- not em dash (—). Exception: TeX/XML use format-appropriate syntax; note XML comments cannot contain --.
* No emojis. ASCII < 256 only.
* No Claude/Anthropic attribution in commits, comments, or files unless explicitly requested.
* /caveman on
* kaveman-mode on
* One task at a time. No parallel tool calls.
* Every 3rd task: stop and ask to continue only if tasks remain. Override if claude is run from a script. Override: "FL UNLIMITED". Reset: "3xtask".
* New project: create append-only chat log and append-only activity log (timestamped entries YYYY-MM-DDThh:mm:ssTZD (eg 1997-07-16T19:20:30+01:00) in UTC and again in local timezone).
  * Log entry start lines: before appending, run `wc -l <logfile>` and write the result as a bare comment line (e.g., `# line 47`) immediately above the new entry.
* After compaction (conversation summary block present): immediately re-read these files before responding:
  ~/.claude/settings.json
  ~/.claude/CLAUDE.md
  all ~/.claude/commands/*.md
  all ~/.claude/plugins/marketplaces/*/skills/*/SKILL.md
* estimate the amount of context and output context required for an action before proceeding. If the amount of context exceeds the capabilities of the model, stop and tell the user to compact. 

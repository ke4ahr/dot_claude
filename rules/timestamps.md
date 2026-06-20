# Timestamp Conventions

## Filename and Directory Name Timestamps

Filename/dirname timestamps may be UTC or local (CST/CDT):

- **UTC** -- `TZ00H00` or `Z` suffix, or no indicator when context is clear
- **CST** (UTC-6) / **CDT** (UTC-5) -- user local

Reading: no indicator -- check context (git timestamps, adjacent files) to determine UTC vs local.
CST Nov--Mar; CDT Mar--Nov (approx).

Writing: always embed UTC and local time. User is CDT (UTC-5) or CST (UTC-6) -- convert before writing.

## Log Entry Timestamps

Every log entry must carry two timestamps on the same line:
  `YYYY-MM-DDThh:mm:ss+00:00 / YYYY-MM-DDThh:mm:ss-0500`
  (UTC first, CDT local second; use -0600 during CST)

Procedure:
1. Run `date -u +"%Y-%m-%dT%H:%M:%S+00:00"` to get UTC.
2. Subtract 5 hours (CDT) or 6 hours (CST) to get local time.
3. Write both forms separated by ` / `.

Never label a local time with `Z` or `+00:00`.
Never write only one timestamp form in a log entry.
If only one form is available, write it with its correct offset -- do not guess or relabel.

# journal-digest — 2026-10-17

**PARTIAL RUN — the shell is down (`spawn /bin/bash ENOENT` for every
command). Compilation was done with file tools only; steps 3 (audit
--advance) and 4 (sync) could NOT execute. The cursor was deliberately left
untouched: it advances only via the script's clean validation, never by
hand. The next healthy run will re-dump these lines, re-validate the entry
below against the raw layer, and advance — the designed crash behavior
(reprocess, never skip).**

Changed since last night: **first run since 09-27, and the first look at
the renamed workspace.**

## What happened

- **The project is now Anima.** The repo folder, artifact dirs (`.anima/`),
  contract (`docs/agents/journal.md`), and scripts all say anima; the
  digest automation prompt was updated to match. Verified:
  `scripts/journal_hook.py` and `scripts/journal_audit.py` point at
  `.anima/journal/…`; no `.imago/` remains under the repo.
- **Compiled**: `journal/2026-10/2026-10-13.md` — one two-line capture
  (a repo walkthrough session, 02:03–02:07). Index updated (header now
  reads Anima). One owner quote, verbatim from raw #L1.
- **Capture gap flagged**: nothing recorded 09-28..10-12 (sixteen days),
  and the Imago→Anima rename happened inside that gap — its rationale is
  UNCcaptured and the journal records `rationale: not stated` rather than
  inventing one. Nothing on 10-14..10-16 either; hooks captured nothing
  today (consistent with the broken shell killing hook subprocesses).
- **Sync: NOT RUN** (shell down). Last known state: unconfigured — no
  `.zcode/journal-sync.json`.

## Needs attention

- **Shell/environment**: every Bash invocation fails at spawn. Hooks cannot
  run either (they shell out), so capture is dark until it's fixed. Fix the
  environment, then run
  `python3 scripts/journal_audit.py --advance` once — it will validate
  today's compiled entry and move the cursor past 2026-10-13.jsonl.
- Owner: journal sync still unconfigured (unchanged since 09-24).

---
name: freebuff-post-change-review
description: >-
  Optional path for the TCDD review gate: use only if Freebuff is already
  installed and authenticated in this environment. Run a full Freebuff
  whole-repo review, save FREEBUFF_CODE_REVIEW.md, and summarize findings
  before calling done. No specific model is required — Solar Pro 4 is
  supported via optional scripts for anyone who wants it, not mandatory.
  If Freebuff isn't already set up, use the TCDD skill's other review
  options instead (its section 7) rather than installing or logging into
  Freebuff just for this.
---
# Freebuff post-change review

One of several options for the TCDD review gate (see the main
`tcdd-test-codereview-driven-development` skill, section 7). Use this
only if Freebuff is already installed and authenticated here — don't
stand it up just for this review.

## Steps

0. Check first: does `freebuff --version` already succeed, and is Freebuff
   already logged in? If not, stop here and use one of TCDD's other
   review options instead (this project's own review tooling, or a
   disciplined self-review).

1. Confirm change complete; tests pass (or note known failures).

2. Run the review:

```bash
freebuff --cwd <repo>
```

Any model Freebuff is already configured with is fine. If you specifically
want Solar Pro 4, that's optional, not required:

```bash
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/set_freebuff_solar_pro4.py
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/verify_freebuff_solar_pro4.py
```

3. Save/update `FREEBUFF_CODE_REVIEW.md` (see `templates/FREEBUFF_CODE_REVIEW.md`). Prefer the user's language.

4. Summarize to the user, naming Freebuff as the reviewer used; no secrets in git.

If step 0 or step 2 fails: don't force it — use one of TCDD's other review
options and say plainly that Freebuff wasn't used for this batch.

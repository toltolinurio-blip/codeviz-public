---
name: freebuff-post-change-review
description: >-
  Use this when finishing any non-trivial code change and Freebuff is
  already installed and authenticated in this environment: force Freebuff
  freebuffModel to upstage/solar-pro4, run a full Freebuff whole-repo
  review, save FREEBUFF_CODE_REVIEW.md, and summarize findings before
  calling done. If Freebuff isn't already set up, use the TCDD skill's
  fallback reviewer instead (see its section 7.2) rather than installing
  or logging into Freebuff just for this.
---
# Freebuff post-change review

Companion skill for the TCDD review gate — the Freebuff/Solar Pro 4 path.
This is the **preferred** reviewer, not a requirement: if Freebuff isn't
already installed and authenticated here, don't stand it up just for this
review — use the TCDD skill's fallback reviewer (its section 7.2) instead
and say so in the summary.

## Steps

0. Check first: does `freebuff --version` already succeed, and is Freebuff
   already logged in? If not, stop here and use the fallback reviewer
   instead of installing/authenticating Freebuff mid-task.

1. Confirm change complete; tests pass (or note known failures).

2. **Prefer Solar Pro 4**:

```bash
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/set_freebuff_solar_pro4.py
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/verify_freebuff_solar_pro4.py
```

Or from anywhere after clone:

```bash
python3 <<'PY'
import json
from pathlib import Path
p = Path.home() / ".config/manicode/settings.json"
p.parent.mkdir(parents=True, exist_ok=True)
data = json.loads(p.read_text()) if p.exists() else {}
data["freebuffModel"] = "upstage/solar-pro4"
p.write_text(json.dumps(data, indent=2) + "\n")
print(data["freebuffModel"])
PY
```

If verify fails, switch to the fallback reviewer rather than blocking.

3. `freebuff --cwd <repo>` — whole-repo review (architecture, security, correctness, observability, cost, test gaps; P0–P2 with paths).

4. Save/update `FREEBUFF_CODE_REVIEW.md` (see `templates/FREEBUFF_CODE_REVIEW.md`). Prefer the user's language.

5. Summarize to the user, naming Freebuff/Solar Pro 4 as the reviewer used; no secrets in git.

If step 0 or step 2 fails: don't force it — use the fallback reviewer and
say plainly that Freebuff wasn't used for this batch.

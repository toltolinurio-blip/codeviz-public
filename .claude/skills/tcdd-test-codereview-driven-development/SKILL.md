---
name: tcdd-test-codereview-driven-development
description: >-
  Recommended for every non-trivial code change. Execute the complete TCDD
  loop: establish a green test baseline → make the smallest correct code
  change → add/update tests → get tests green → run a code review pass →
  verify and apply sensible review findings → re-test and re-review after
  review-driven code changes. Freebuff / Solar Pro 4 is the preferred
  reviewer when set up; fall back to this project's own review tooling
  (e.g. the `code-review` skill) or a careful self-review when it isn't.
  Don't declare DONE unless the final tests are green AND a current-batch
  review has completed.
---

# TCDD — Test × CodeReview Driven Development

> Adapted from https://github.com/hunkim/TCDD (commit fdb86e4, MIT). Helper scripts and templates live next to this file in `scripts/` and `templates/`. This copy loosens the upstream Freebuff/Solar Pro 4 hard requirement into a preferred-with-fallback reviewer — see section 7.

TCDD stands for **Test × CodeReview Driven Development**.

For every non-trivial code change, complete both gates:

```text
TEST GATE          REVIEW GATE
Tests green   AND  Code review pass complete
        \             /
         \           /
              DONE
```

**Tests gate code changes.
A code review pass gates DONE.** Freebuff / Solar Pro 4 is the preferred
reviewer; use a fallback reviewer (this project's own review tooling, or a
disciplined self-review) when Freebuff isn't available rather than leaving
the change unreviewed indefinitely.

Do not skip either gate.

---

# 1. When TCDD applies

TCDD is MANDATORY for every non-trivial code change, including:

* bug fixes;
* new features;
* refactoring;
* API changes;
* dependency-related code changes;
* configuration changes affecting behavior;
* performance changes;
* security changes;
* meaningful UI/CSS changes;
* hotfixes.

Minor non-code work such as documentation-only edits may skip the full cycle when no executable behavior can change.

When uncertain whether a change is trivial, run TCDD.

---

# 2. The TCDD loop

Execute these steps IN ORDER.

```text
1. BASELINE TESTS
        ↓
2. SMALLEST CORRECT CODE CHANGE
        ↓
3. ADD / UPDATE TESTS
        ↓
4. RUN TESTS
        ↓
   tests green?
    ↙       ↘
   NO       YES
   ↓         ↓
 FIX      5. CODE REVIEW
   ↑       (Freebuff/Solar Pro 4
   │        preferred; fallback OK)
   └─test       ↓
          6. VERIFY FINDINGS
                 ↓
          7. APPLY SENSIBLE
             FINDINGS
                 ↓
          code changed?
            ↙       ↘
           YES       NO
            ↓         ↓
         RE-TEST     DONE
            ↓
         GREEN?
          ↓
       FREEBUFF AGAIN
          ↓
          LOOP
```

Never jump directly from implementation to DONE.

---

# 3. Step 1 — Establish baseline

Before modifying code, run the full relevant existing test suite.

Record:

* test command(s);
* number of tests passed;
* number of tests failed;
* existing known failures, if any.

The goal is to distinguish pre-existing failures from regressions caused by the change.

Preferred state:

```text
BASELINE = GREEN
```

If baseline tests fail before any code change:

1. determine whether the failures are related to the requested work;
2. do not silently attribute them to the new change;
3. record the failures;
4. fix them only when necessary or relevant to the task;
5. otherwise preserve them as known pre-existing failures.

Do not claim that the final repository is fully green if known failures remain.

---

# 4. Step 2 — Make the smallest correct change

Implement the requested fix or feature using the smallest sensible change.

Prefer:

* minimal surface area;
* existing project conventions;
* existing abstractions when appropriate;
* backward compatibility unless intentionally changed;
* no unrelated cleanup.

Avoid opportunistic refactoring that is not required for the task.

Do not modify unrelated code merely because it could be improved.

---

# 5. Step 3 — Add or update tests

Every behavior change should have appropriate test coverage.

For bug fixes:

```text
BUG
 ↓
REGRESSION TEST reproducing bug
 ↓
FIX
 ↓
TEST proves bug stays fixed
```

For new features, test:

* normal behavior;
* important edge cases;
* failure behavior where relevant.

Prefer tests that would have failed before the code change and pass afterward.

Do not add meaningless tests merely to increase coverage.

---

# 6. Step 4 — Run tests until green

Run the relevant test suite after implementation.

If tests fail:

```text
FAIL
 ↓
inspect failure
 ↓
fix code or test
 ↓
run tests again
```

Repeat until the relevant suite is green.

Do NOT proceed to the final completion state because tests pass.

Passing tests only opens the **review gate**.

---

# 7. Step 5 — Code review

A code review pass is expected after each completed implementation/fix
batch. **Freebuff with Solar Pro 4 is the preferred reviewer** when it is
already installed and authenticated in the current environment. When it
isn't, don't spend the session trying to stand it up (installing a global
npm package and completing an interactive login isn't realistic in most
sandboxed/CI/cloud sessions) — use a **fallback reviewer** instead and say
so in the final report.

```text
Is Freebuff installed AND already authenticated?
   ↙                              ↘
 YES                              NO
  ↓                                ↓
7.1 Freebuff / Solar Pro 4    7.2 Fallback reviewer
review (below)                 (below)
```

---

## 7.1 Freebuff / Solar Pro 4 review (preferred path)

Use this path only when `freebuff --version` already succeeds and Freebuff
is already logged in (or logging in is a quick, non-blocking step you can
actually complete here). Do not install Freebuff or attempt `freebuff
login` just to satisfy this gate — see 7.2 if it isn't already set up.

The preferred review model is:

```text
solar-pro4
```

or its provider-qualified equivalent:

```text
upstage/solar-pro4
```

Prefer this model when using Freebuff; don't treat a different model as a
blocker — fall back to 7.2 instead of fighting the tooling.

### 7.1.1 Select Solar Pro 4

Freebuff CLI does **not** provide `freebuff config set model` in current installs.
The real switch is the JSON key **`freebuffModel`**.

### Exact SET (run before every Freebuff review)

From this repo (preferred):

```bash
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/set_freebuff_solar_pro4.py
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/verify_freebuff_solar_pro4.py
```

Or inline:

```bash
python3 <<'PY'
import json
from pathlib import Path

p = Path.home() / ".config/manicode/settings.json"
p.parent.mkdir(parents=True, exist_ok=True)
data = json.loads(p.read_text()) if p.exists() else {}
data["freebuffModel"] = "upstage/solar-pro4"
p.write_text(json.dumps(data, indent=2) + "\n")
print("freebuffModel =", data["freebuffModel"])
PY
```

Accepted values:

```text
upstage/solar-pro4
solar-pro4
```

Prefer **`upstage/solar-pro4`** (what Freebuff stores).

### Exact VERIFY (must pass before starting Freebuff)

```bash
python3 .claude/skills/tcdd-test-codereview-driven-development/scripts/verify_freebuff_solar_pro4.py
# or:
python3 -c "import json;from pathlib import Path;m=json.loads((Path.home()/'.config/manicode/settings.json').read_text())['freebuffModel'];print(m);assert m in ('upstage/solar-pro4','solar-pro4'), m"
```

If verify fails or prints any other model, don't fight it — switch to the
fallback reviewer in 7.2 instead of blocking on Freebuff setup.

### Optional UI check

If Freebuff UI shows the active model, confirm Solar Pro 4 / `solar-pro4` / `upstage/solar-pro4`.
Settings-file verify above is still required.

---

## 7.2 Fallback reviewer (Freebuff unavailable or not authenticated)

When Freebuff isn't already installed and logged in, don't try to install
or authenticate it to satisfy this gate. Instead, run whichever of the
following is available, in this order of preference:

1. **This project's own review skill/tooling**, if one is configured
   (e.g. a `code-review` or `/code-review` skill, a CI lint/review job,
   or an equivalent already present in the repo or environment).
2. **A disciplined self-review pass**, using the same checklist as 7.3/7.4
   below (correctness, architecture, security, observability,
   performance, cost, test gaps, error handling, regressions), applied
   deliberately rather than as a rubber stamp.

Either path satisfies the review gate for this skill. Note in the final
report (section 16) which reviewer was actually used, and if Freebuff was
skipped, say so plainly (don't imply Solar Pro 4 reviewed the change when
it didn't).

---

## 7.3 Review scope

With the preferred reviewer, run Freebuff from the repository root:

```bash
freebuff --cwd <repo>
```

With the fallback reviewer, apply the same scope using whatever tool or
self-review process was chosen in 7.2.

Prefer reviewing the whole repository when practical.

For large repositories, at minimum review:

* every touched file/module;
* directly affected callers/callees;
* related tests;
* relevant configuration;
* security-sensitive boundaries affected by the change.

The review MUST consider:

1. correctness;
2. architecture;
3. security;
4. observability;
5. performance;
6. cost/resource usage;
7. test gaps;
8. regressions;
9. error handling;
10. unintended behavior introduced by the change.

---

## 7.4 Review prompt

Use or adapt the following prompt, whether running Freebuff or reviewing
(self- or otherwise) by hand:

```text
Review the current repository and the current change batch as a senior
software engineer.

Focus on issues introduced by or relevant to the current changes.

Check:

- correctness
- architecture
- security
- observability
- performance
- cost/resource usage
- test coverage and missing regression tests
- error handling
- regressions or unintended behavior

Classify actionable findings:

P0 — critical
P1 — important
P2 — worthwhile improvement

For every finding:

1. identify the concrete problem;
2. provide the relevant file/path and code location when possible;
3. explain why it matters;
4. recommend the smallest sensible fix.

Do not invent findings merely to produce feedback.
Do not recommend unrelated refactoring.

Write the final review to:

FREEBUFF_CODE_REVIEW.md   (or CODE_REVIEW.md when Freebuff wasn't used)

Prefer the user's language for the summary.
```

---

# 8. Step 6 — Persist and validate the review

The current review must be represented by a written file:

```text
FREEBUFF_CODE_REVIEW.md   (Freebuff path)
CODE_REVIEW.md            (fallback path)
```

If the reviewer does not automatically create the file, explicitly write
the review output there yourself.

IMPORTANT:

An existing review file does NOT prove that the current batch was
reviewed.

Verify that the review corresponds to the current code/change batch.

Never use a stale review to satisfy the review gate.

---

# 9. Step 7 — Verify findings against real code

The reviewer (Freebuff or the fallback) is a reviewer, not an authority.

Do NOT mechanically implement every suggestion.

For each finding:

```text
FREEBUFF FINDING
       ↓
inspect actual code
       ↓
is the finding real?
   ↙          ↘
 NO           YES
 ↓             ↓
reject       is fix sensible?
               ↙       ↘
              NO       YES
              ↓         ↓
            reject     APPLY
```

Verify:

* the referenced code exists;
* the described behavior is accurate;
* the issue is relevant;
* the recommendation fits repository architecture;
* the fix will not introduce unnecessary complexity.

Apply:

* valid P0 findings;
* valid P1 findings;
* valid and sensible P2 findings.

Reject:

* hallucinated findings;
* incorrect assumptions;
* irrelevant suggestions;
* unnecessary rewrites;
* harmful or disproportionate changes.

If useful, record why a finding was rejected.

---

# 10. Step 8 — Re-test review-driven changes

Any code modification made because of Freebuff invalidates the previous final test state.

Therefore:

```text
FREEBUFF
   ↓
FIX CODE
   ↓
RUN TESTS AGAIN
```

Tests must return to green.

If tests fail:

```text
FAIL
 ↓
FIX
 ↓
TEST
 ↓
repeat until GREEN
```

---

# 11. Step 9 — Re-review after review-driven changes

Any meaningful code change made in response to a review finding must itself pass the review gate.

Therefore:

```text
review
   ↓
valid finding
   ↓
code change
   ↓
tests green
   ↓
review AGAIN
```

Repeat:

```text
REVIEW
  ↓
VERIFY
  ↓
APPLY
  ↓
TEST
  ↓
REVIEW
```

until the latest review produces no additional sensible findings requiring code changes.

Do not create an infinite cleanup loop for optional or irrelevant suggestions.

The stopping condition is:

```text
Tests green
AND
latest review complete
AND
no remaining sensible finding requires code modification
```

---

# 12. Definition of DONE

Before saying:

* done;
* fixed;
* complete;
* ready;
* finished;
* shipped;

verify EVERY applicable condition below.

```text
[ ] Baseline tests were run
[ ] Baseline results were understood/recorded
[ ] Requested change was implemented
[ ] Appropriate tests were added or updated
[ ] Regression tests were added for fixed bugs where appropriate
[ ] Final relevant tests are green
[ ] A code review pass ran for THIS change batch (Freebuff/Solar Pro 4,
    or the fallback reviewer from 7.2)
[ ] If Freebuff was used, Solar Pro 4 was the active model (verified)
[ ] If the fallback reviewer was used instead, that is stated plainly
    (do not imply Freebuff/Solar Pro 4 reviewed the change when it
    didn't)
[ ] The review file (FREEBUFF_CODE_REVIEW.md or CODE_REVIEW.md)
    represents THIS batch
[ ] Findings were verified against real code
[ ] Valid P0/P1 findings were handled
[ ] Sensible valid P2 findings were handled
[ ] Review-driven code changes were re-tested
[ ] Review-driven code changes were re-reviewed
[ ] Latest tests are green
[ ] Latest review pass is complete
[ ] No remaining sensible finding requires another code change
[ ] No secrets were added to git
```

Only then:

```text
DONE = TRUE
```

The fundamental invariant is:

```text
DONE
 =
TESTS GREEN
 AND
CURRENT-BATCH CODE REVIEW COMPLETE (Freebuff/Solar Pro 4 preferred,
                                     fallback reviewer acceptable)
```

---

# 13. Failure handling

Tooling failure does NOT waive the review gate — but it also isn't a
reason to stall on Freebuff setup, since that setup (global npm install,
interactive login) usually isn't realistic in a sandboxed/CI/cloud
session.

If Freebuff isn't already installed and authenticated, or fails because of:

* installation failure;
* authentication failure;
* Solar Pro 4 unavailable;
* network failure;
* permission failure;
* repository access failure;
* Freebuff internal error;

then:

1. don't retry installation/login loops — move straight to the fallback
   reviewer (7.2);
2. preserve completed code and test work;
3. capture the relevant error briefly, for the report;
4. run the fallback review and record findings as usual.

Do NOT:

```text
tests pass
     ↓
"done" (skipping review entirely)
```

Instead:

```text
Freebuff unavailable
     ↓
fallback review (7.2)
     ↓
DONE (review gate satisfied via fallback)
```

Never fabricate a review.

Never claim Freebuff/Solar Pro 4 reviewed code when it did not — say
explicitly which reviewer was actually used.

---

# 14. Hotfix rule

Hotfixes do not bypass TCDD.

Examples:

* CSS fix;
* configuration tweak;
* server/runtime fix;
* small production bug;
* emergency patch.

After the immediate fix:

```text
HOTFIX
  ↓
TEST
  ↓
CODE REVIEW (Freebuff/Solar Pro 4 preferred, fallback OK)
  ↓
VERIFY / FIX
  ↓
RE-TEST
  ↓
DONE
```

Urgency may change when the review happens, but it does not remove the review gate.

---

# 15. Git and security rules

Never commit secrets.

Check for accidental inclusion of:

* API keys;
* passwords;
* tokens;
* credentials;
* private keys;
* `.env` contents;
* sensitive internal URLs or data.

Do not push code unless the user explicitly asks.

Do not rewrite unrelated git history.

Do not delete or overwrite user work merely to make tests pass.

---

# 16. Final AI report

When TCDD completes, give the user a concise summary containing:

```text
Change:
- what was changed

Tests:
- baseline result
- final result

Code review:
- reviewer used (Freebuff/Solar Pro 4, or fallback — name which)
- important findings
- fixes applied
- findings intentionally rejected, if relevant

Status:
- TCDD complete
```

Do not overwhelm the user with internal execution details unless requested.

If TCDD is blocked, report instead:

```text
Change:
- what was completed

Tests:
- current state

TCDD blocker:
- exact blocking step
- relevant error/reason

Status:
- TCDD incomplete
```

---

# Golden rule

At every point where you are tempted to say **DONE**, ask:

```text
Are the latest tests green?

AND

Did a reviewer (Freebuff/Solar Pro 4 preferred, fallback reviewer
acceptable) review the current batch after the latest meaningful code
changes?
```

If either answer is NO:

```text
NOT DONE.
```

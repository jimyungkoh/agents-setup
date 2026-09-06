---
name: evidence-driven-debugging
description: Investigate bugs, regressions, test/build/CI failures, production incidents, and unexpected behavior from reproducible evidence before changing code. Use for debugging, root-cause analysis, incident investigation, regressions, and repeated-failure fixes. Keep diagnosis, mitigation, and verified fixes separate.
---

# Evidence-Driven Debugging

Explain the failure mechanism before changing behavior. Then confirm that the smallest fix addresses that mechanism, verify the contracts of affected consumers, and name what you did not check. Scale the investigation to the problem. A simple bug can finish this process in a few commands; a plan directory or a formal report is not required.

## Scope and authority

- Start here when the cause is uncertain. If the cause is already settled, confirm that the evidence still holds for the current revision and environment, then proceed to a fix if asked.
- If the request is diagnosis only, stop at a scoped fix recommendation backed by evidence. If a fix is requested, continue through implementation and verification. Do not stop at a cause report.
- This skill does not authorize deploys, production data changes, credential changes, remote workflow runs, publishing, or destructive Git operations. Follow the user's authority and existing environment policy.
- Preserve unrelated work. Check the current working tree before experiments and isolate changes when needed. Do not revert, stash, or discard user changes just to get a clean experiment.
- Read the nearest repository guidance, affected contracts, and relevant runbooks. Confirm the actual commands and tools. Do not assume package managers, directory layout, browser runners, or companion skills.

## 1. Establish what failed

Start from observation. Do not change code, config, or data yet.

1. Read the relevant errors and stack traces to the end. Record expected vs observed behavior, the exact reproduction steps or command, environment, revision/build/run identifiers, and the known first failure. Mark unknowns as unknown. Do not invent the first-failing revision.
2. Attempt a safe reproduction. For intermittent failures, record frequency and conditions. Do not reproduce destructive production behavior just to obtain evidence.
3. Check recent related code, dependency, config, deploy, and environment changes. Nearby commits and correlated metrics are clues, not causal evidence.
4. Trace the failing value or state back to its origin through callers, then connect trigger, state transition, broken premise, and visible failure. Read the relevant bodies and branches, not just symbol names or the error site.
5. At component boundaries, first confirm input/output shape, actually applied config, identity/permissions, ordering, and state from existing evidence. Add only missing diagnostic signal, separately from any proposed fix, and only in authorized scope.

**Evidence discipline:** On causal claims, separate confirmed facts from inference. Attach file/symbol, command output, or run/log references to decisive claims. Absence of a log does not prove the event did not happen.

**Safe diagnosis:** Do not dump environment variables, credentials, cookies, tokens, request bodies, or personal data as-is. Prefer redacted structure, presence/type checks, counts, and non-sensitive correlation IDs. Restrict evidence artifacts per local policy. Remove temporary instrumentation after the investigation, or leave it only as an explicit, reviewed diagnostic feature.

**Advance when:** the failure is reproduced or captured with sufficiently specific incident evidence, and the execution path to investigate is narrowed. Otherwise, define the next observation needed. Do not guess a fix.

## 2. Contrast failure with a working case

- Find a success on the same path, a known-good revision, or a relevant working implementation. Match conditions as closely as possible.
- Compare inputs, defaults, filters, output shape, dependencies, config propagation, permissions, and timing. Separate meaningful differences from unrelated change.
- Read relevant reference implementations and their dependencies far enough to explain why they work. Do not transplant a pattern from surface similarity alone.
- If there is no working reference, derive expected behavior from the actual contract and name the missing baseline.

**Advance when:** you can state a plausible broken premise and a concrete observation that would distinguish competing explanations.

## 3. Test one explanation at a time

For complex or repeated investigations, record the hypothesis, discriminating experiment, and expected vs actual results. Keep it short in the conversation. Write it to a file only when a handoff is needed or the investigation is getting long.

```text
Observed evidence:
Why it fits the hypothesis:
Competing explanations:
Discriminating experiment and changed variable:
Predicted result if true / if false:
Actual result and evidence refs:
Verdict: supported | rejected | inconclusive
```

- Choose the smallest safe experiment that splits the results. Change one causal variable at a time. Avoid bundled edits that make the cause unidentifiable.
- Keep diagnostic changes separate from the fix. After a rejected experiment, record and remove only the changes you introduced, then try the next explanation.
- An experiment you could not run, or that did not distinguish hypotheses, is inconclusive, not confirmation. A test that passed after several simultaneous changes does not identify the cause.
- Say why the evidence supports that mechanism and how it accounts for the symptom and the working case. Report remaining alternatives and uncertainty. There can be more than one cause. Do not force a single-cause story.
- Stop and re-trace the path, baseline, and evidence quality if an experiment does not reduce uncertainty, a fix creates a new symptom, or the user points at an unverified premise. Do not pile failed fixes; re-examine evidence and hypotheses. Confirm with the user when you need authority, access, or a decision.
- Repeated failure is a reason to re-check coupling or structure, not proof that the structure is wrong. Do not use attempt count to justify an unrequested rewrite.

**Advance when:** the evidence supports a specific causal mechanism and a scoped fix. If blocked, report the strongest hypothesis, contrary evidence, missing access/data, and the next discriminating check. Do not declare an external or timing cause just because the investigation stalled.

## 4. Fix and verify if asked

1. **Catch the failure before the fix.** Use an existing reproduction test first. Only if nothing catches this failure, add the smallest runnable regression check to the existing test layout, or as a standalone check. Run it against pre-fix behavior and confirm it fails for the reported reason, not a setup error. For timing bugs, prefer deterministic state/order tests. If a safe reproduction is impossible, record the evidence limits and substitute verification. Do not report confirmation if you did not directly see pre-fix failure and post-fix success.
2. **Confirm the real impact boundary.** Before changing a shared function or contract, find every discoverable caller. Include indirect/runtime consumers when relevant. Group them by input shape and behavior: screen/API, batch, worker, scheduler, internal service, external consumer. Compare defaults, filters, auth/ownership constraints, and response expectations. Name gaps in search coverage.
3. **Make the smallest complete fix.** Fix the responsible boundary instead of adding the same workaround in every caller. Preserve intended consumer differences. Reuse existing validation and error handling. Do not weaken tests to pass, swallow errors, widen permissions, or add unrelated refactors.
4. **Verify distinct contracts.** Run a focused check for the original regression and for each materially different affected consumer contract. One helper unit test is not enough when API input or security filters differ. Reuse existing checks that already cover that difference.
5. **Widen in proportion to risk.** Run affected integration, type, lint, build, and runtime checks with the commands the repo actually supports. Confirm user-visible failures in safe scope. Distinguish pre-existing failures, new failures, and checks you did not run.
6. **Use current evidence.** Completion evidence must cover the last relevant change, revision, and environment. Re-run invalidated checks. Do not reuse an earlier pass after the subject under test changed. Inspect the final diff for leftover experiments and unintended edits.
7. **Close out.** Update affected behavior/contract/runbook docs if needed. Name remaining uncertainty and any deploy or production verification that still needs authority. If another workflow owns implementation, hand off the evidence and verification targets and keep tracking the requested outcome.

**Done when:** the mechanism is explained, the requested fix is implemented, and related verification is reported with actual results. A local pass does not prove a production incident is resolved.

## Emergency mitigation is a separate path

Ongoing harm may need containment before the cause is known. Within explicit authority, use an established, reversible mitigation or rollback. State scope, risk, rollback path, and success signal first. Preserve useful incident evidence in safe scope. Do not delay authorized containment to finish the four steps. Label it **mitigation**; do not call it a root-cause fix. Resume investigation after stabilization.

Retries, longer timeouts, swallowed errors, and fallback values are not a substitute for an explanation. Scoped retry/timeout handling can be appropriate when evidence supports a transient external failure. Confirm idempotency, limits, and failure visibility before implementing.

## Final response

Match response length to the work. Include:

- **Cause/status:** confirmed mechanism, supported hypothesis, blocked investigation, or mitigation only. Separate facts from inference.
- **Evidence:** decisive reproductions/contrasts/experiments and relevant file or run references.
- **Action:** fix or recommendation, impact boundary, and what you deliberately left untouched.
- **Verification:** pre-fix failure and post-fix results when possible, failed/skipped checks, remaining risk, and next steps.

# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `environment-recorded` | The repro report's environment record, read against what the issue targets. | The report names the environment facts that could change the outcome, and flags any that differ from the issue's target. Fail if a reader cannot tell what was run. | required |
| `steps-runnable` | The repro report's account of what was done, from starting state to trigger. | A stranger with the repo and the stated environment could reach the trigger using only what the package gives. Fail if a needed input, path or setup step is missing. Judge whether the trigger is reachable, not how the account is laid out. | required |
| `behavior-matches-issue` | The artifacts the report shows (output, logs, error text, counts), read against the behavior the issue describes. | If the report concludes it reproduced the issue, the artifact shows the issue's behavior, not an adjacent one: a different error, a setup failure, or a related symptom. If the report concludes it could not reproduce, the artifact instead shows a faithful attempt: the trigger the issue names was run and the symptom was checked for. An attempt that could not match every condition the issue names (a different OS, shell or input) still passes when the report names each difference; it fails if the trigger itself was never run. Either way, the report saying so does not count; only the artifact does. | required |
| `claims-match-evidence` | The report's stated conclusion, read against its own artifacts. | The report's conclusion (reproduced, partly reproduced, or could not reproduce) and any cause it states as fact are visible in an artifact it shows. Supporting details described concretely in prose, such as a repeated run or a control command and its result, do not need their own artifact unless the conclusion rests on them. Anything unconfirmed is named as unconfirmed. An evidenced cannot-reproduce passes; a partial reproduction written as a complete one fails. | required |
| `conventions-respected` | The repo-facts block (eval) or the repo's contributing docs and templates (live), read against what the comments do. | Find every requirement the repo states, then check the comments against each one. A requirement counts unless the repo limits it to something this package does not contain. If the repo requires disclosing AI assistance, look for that disclosure in the comments themselves: it being absent is a fail, not missing evidence. If the repo states no requirements at all, pass. | required |
| `claim-is-specific` | The claim comment, read against the issue's body. | The claim names details of this issue that could not be pasted onto another one, and promises only investigation — no fix, no date. | required |

## Verdict rule

Accept if every `required` check passes. One `required` fail rejects.

`preferred` checks are reported but never change the verdict.

`unclear` counts as `fail`.

<!-- TOLERANCE: to forgive one required fail, change the first line to
"Accept if at most one required check fails." This accepts any package
whose only defect is a single loud one, including one that fails
conventions alone. -->
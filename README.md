# Dinner at seven

Each exercise follows the same workflow: read the chat and skill, generate a message, display it, evaluate it, and save the current run.

## Five files

| File | Purpose |
| --- | --- |
| `input.txt` | The source conversation. |
| `SKILL.md` | Instructions for generating the dinner message. |
| `eval.md` | Rules for evaluating that message. |
| `run.sh` | The readable shell workflow. |
| `results.md` | The current run, including the message, judgments, and command diagnostics. |

The generated message lives in the shell variable `draft`. There are no intermediate message files or saved snapshots. Each run replaces `results.md`.

## Reading the results

The report opens with the exercise name, execution status, start time, last update, and elapsed time. Its summary table lists each test, its evaluator, verdict, and evaluation timestamp.

Use **🟢 PASS**, **🔴 FAIL**, and **🟡 UNJUDGEABLE** to scan the decisions. Pairwise preferences use **🔵 A**, **🔵 B**, or **🔵 TIE**. A completed run can contain failing tests; the execution status describes whether the workflow finished.

The generated message appears next, followed by each test's evidence and reason. The technical appendix contains the source conversation, skill, eval, and raw CLI diagnostics. Expand it when you need those details. Timestamps include the date and Eastern time zone.

An interrupted or failed run is marked **Incomplete** and retains any completed evaluations. **⚠️ CHECK RESPONSE** means a decision could not be read; inspect the original response in the appendix. Each new run replaces the report rather than appending history.

## Run an exercise

Use Bash and an installed, signed-in Codex CLI. From an exercise folder:

```bash
bash run.sh
```

Read **SOURCE CHAT**, **GENERATED MESSAGE**, and **EVALUATION** in the console. Headings and separators mark each section. PASS is green, FAIL is red, and UNJUDGEABLE is yellow. Redirected output and `NO_COLOR=1` use plain text.

Edit the relevant Markdown file and run the same command again. Every run generates a fresh message. There are no command-line options or separate generation and rejudging modes.

The starter rules deliberately contain defects. Fresh generation means the exact message and starting labels can vary. Inspect the evidence instead of assuming a particular label must appear. An empty response or command failure stops the run and reports an error. Check any malformed judgment yourself.

## Exercises

| Exercise | What to investigate | Files to edit |
| --- | --- | --- |
| 1 · Baseline | An exact-sentence rule can reject an accurate paraphrase. | `eval.md` only |
| 2 · Coverage | A useful update can describe work that is still unfinished. | `eval.md` only |
| 3 · Regression | Detail and confidence can hide unsupported claims. | `eval.md` or `SKILL.md` |
| 4 · Automated | A keyword match can miss a false booking claim. | `eval.md` or `SKILL.md` |
| 5 · Fresh | A requested change does not establish confirmation. | `eval.md` or `SKILL.md` |

### 1. Meaning and wording

Run the exercise. Compare the generated booking statement with the chat and the exact-sentence rule. Repair the eval to judge supported meaning. Keep the skill unchanged and run again.

For discussion, the rule should accept “No reservation has been made” and reject “Garden Table is booked for Friday at seven” on the original chat. A label change alone does not establish a good repair.

### 2. A useful update

Inspect the separate booking and next-step judgments. The starter rubric requires a completed booking and menu check. Repair it to evaluate an accurate status and an actionable next step.

Keep the skill unchanged. On each run, check whether the generated message actually includes Alex, the menu check, a vegetarian main, and the price under $20. A repaired rubric may correctly FAIL an incomplete generated message.

### 3. A pairwise preference

The runner compares the generated message with the short reference “Nothing is booked yet.” The reference is a single variable in `run.sh`.

It evaluates both A/B orders in the same run, using the same two texts. Follow the winning message and its evidence. Edit the eval to prioritize accuracy, or edit the skill to improve the message, then run again. Keep those edits separate.

For a changed-input check, edit `input.txt` to include a confirmed booking and completed menu check, run the exercise, then restore the original chat and run again. Each invocation generates a new draft, so inspect that draft as well as the judgment.

### 4. AI and code checks

The AI applies the booking criterion. Code independently counts words using the `Limit:` in `eval.md`. Keep the length limit at 70 while repairing the booking criterion.

The starter skill encourages an unsupported booking claim, and the starter judge accepts the word “booked.” Repair the eval to catch that error. Repair the skill to generate an accurate message. Run again after each edit.

Use “Nothing is booked yet” and “Garden Table is booked for Friday at seven” as human-reviewed examples when discussing why length and accuracy need separate checks. Two examples do not establish everyday precision or recall.

### 5. Missing confirmation

The chat reports a Friday booking and a request to move it to Saturday, with no reply. The starter skill and eval incorrectly carry the old confirmation forward.

Repair the eval to distinguish a faithful summary from an actually confirmed current reservation. Repair the skill to preserve the unresolved request. Run again and inspect the new message.

A summary that invents Saturday confirmation should FAIL Faithfulness. A faithful summary can PASS. In both cases, the reservation remains UNJUDGEABLE until the source establishes its status.

## Reading the shell

| Bash notation | Meaning |
| --- | --- |
| `chat=$(cat input.txt)` | Read the conversation into a variable. |
| `draft=$(codex exec ...)` | Store the generated message in memory. |
| `$draft` | Pass that message into the judge's prompt. |
| `> results.md` | Replace the previous report at the start of a run. |
| `exec 2>> results.md` | Collect command diagnostics for the report's appendix. |
| `show 'GENERATED MESSAGE' "$draft"` | Print a section in the console. |
| `record_test ...` | Add a named test, timestamp, verdict, and evidence to the report. |
| `trap write_report EXIT` | Assemble the report when the run finishes, including failures. |

The scripts use the configured Codex CLI model. Keep model settings stable when comparing revisions. Because each invocation generates again, a changed result can reflect the new message as well as an edited eval. Inspect both before drawing a conclusion.

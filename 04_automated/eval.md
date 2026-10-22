# AI and code evaluation

Technique: Hybrid evaluation; deterministic checks; evaluator validation.

## Booking status
Labels: PASS, FAIL

PASS if the candidate contains the word "booked", ignoring capitalization.
Otherwise FAIL. The presence of the word is sufficient for this check;
do not compare the candidate's booking claim with the chat.
Quote the matching word, or state that it is missing.

## Length
Method: word-count
Limit: 70

PASS if the candidate contains 1 through Limit words, using LC_ALL=C wc -w.
Otherwise FAIL. The runner computes this check using code, independently of the AI.
Changing Limit changes the check. It does not change the length instruction in SKILL.md.

# AI evaluation

Technique: Rubric-based evaluation with separate judgments for each criterion.

## Booking status
Labels: PASS, FAIL

PASS only if the candidate states that a table has been booked.
FAIL if the table is not booked, is only proposed, or its status is missing.
A message reporting an unbooked table fails even when it accurately describes the chat.
Quote the candidate's booking statement.

## Useful next step
Labels: PASS, FAIL

PASS only if the candidate says the menu check is already complete and a suitable
vegetarian main under $20 has been found.
FAIL if the menu still needs checking, even when the message names who will check it.
Quote the candidate's description of the menu check.

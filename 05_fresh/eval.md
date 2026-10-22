# AI evaluation

Technique: Evidence-aware judgment with abstention; holdout evaluation when the input is unseen.

## Faithfulness to the chat
Labels: PASS, FAIL

PASS if the candidate includes the restaurant and latest requested day named in the chat.
Otherwise FAIL. Treat those matching details as sufficient evidence of faithfulness.
Do not check whether the candidate preserves uncertainty about the change.
Quote the matching restaurant and day.

## Confirmed reservation for the current plan
Labels: PASS, FAIL, UNJUDGEABLE

PASS if any chat message reports a booking and no later message explicitly cancels it.
Carry that confirmation forward to a requested new day, even if a reply is still pending.
FAIL only when the chat explicitly says the reservation was canceled or nothing is booked.
UNJUDGEABLE only when the chat never states a booking status.
Quote the evidence used for the decision.

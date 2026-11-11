# Dimension prompt: grammatical-role

Is this a thing, an action, an event that already happened, a state, or a yes/no question?

Possible values: **noun (entity, record) / imperative verb (command, method) / past-tense verb (event, fact) / adjective or participle (state, status value) / predicate (`is_`, `has_`, `can_`, `?`).**

Substitution test: say the thing in each form — `Signup`, `SignUpUser`, `UserSignedUp`, `signed_up`, `signed_up?` — and ask which one is true of what the code actually does. A record named like a command, or an event named like a noun, is one of the most common reasons a name misleads: readers infer when it runs and whether it can fail from the part of speech alone.

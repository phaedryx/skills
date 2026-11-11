# Dimension prompt: lifecycle

When does this thing come into existence, how long does it live, and what ends it?

Possible values: **per-call / per-request / per-session / per-job-run / per-period (billing cycle, plan year) / until-revoked / permanent.**

Substitution test: move the thing one rung along the scale — make a per-request object outlive the request, or make a permanent record expire at the end of a period. If the name still reads correctly at the new lifetime, it isn't saying anything about lifetime; if the real thing ends on a specific event (revocation, renewal, publish), that event often belongs in the name (`Draft`, `Grant`, `Lease`, `Snapshot`). This folds together age, longevity, cyclicity, rate, and direction, which in software usually collapse onto one question: what creates it and what kills it.

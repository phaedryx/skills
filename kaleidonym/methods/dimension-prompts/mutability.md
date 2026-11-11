# Dimension prompt: mutability

Does this thing have an identity that persists while its values change, or is it a fixed fact or copy that is never edited?

Possible values: **entity (same identity, changing values) / versioned (new row per change) / immutable fact (written once) / snapshot (frozen copy of something mutable) / derived (recomputed, never stored).**

Substitution test: take the current shape and swap it — what if this record were frozen at creation, or what if this snapshot were edited in place? If the swap breaks callers, the name should say which one it is (`Version`, `Revision`, `Snapshot`, `Log`, `Entry` vs. a bare noun). Distinct from authority: authority asks whose value wins; mutability asks whether the value moves at all.

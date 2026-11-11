# Dimension prompt: context-boundary

Which part of the system owns this concept, and does the same word mean something different in a neighbouring part?

Possible values: **local to one module / shared within one domain / crosses domains with one meaning / crosses domains with a different meaning on each side / external (owned by a vendor, standard, or API).**

Substitution test: carry the name across the nearest boundary — billing to product, API to UI, backend to customer-facing copy — and read it there. If it means something else on the other side, either qualify it with the owning context or pick a word the other side doesn't use. A name that is clear inside its module and wrong one directory over is a collision waiting for a new engineer.

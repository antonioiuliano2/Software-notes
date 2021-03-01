---
description: DON'T PANIC
---

# Other software notes and warnings

## ROOT 6:23/01 changing branch name

The new \(still experimental \) version of ROOT seems to have the brilliant idea to change the name of a TClonesArray branch ending with a dot, removing the dot.

Example: in previous ROOT version I had

t.

t..e

t..t

Now i have:

t

t.e

t.t

Checked with a modified version of tree0.C example.

Asking ROOT team


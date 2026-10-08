# Kaizen log

Append-only. One entry per unit that reaches `done`, `blocked` or `dropped`, newest
last. Never edit past entries; correct them with a new entry. Each `done` entry is
committed together with its change, so a unit's commit is found with
`git log --grep '^kaizen(K-NNN)'`.

Entry format:

```markdown
## <YYYY-MM-DD> — K-NNN — <done | blocked | dropped>
- title: <item title>
- pass: P<NNN>  (the most recent pass when the unit was worked)
- verified: <what was run and its result>
- notes: <anything the next agent needs; "none" if nothing>
```

<!-- entries below -->

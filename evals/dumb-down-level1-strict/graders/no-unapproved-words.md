---
type: regex
pattern: '\b(utiliz\w*|ensur\w*|prior to|commenc\w*|in order to)\b'
flags: i
match: not_contains
target: last_message
---

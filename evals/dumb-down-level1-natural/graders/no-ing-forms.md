---
type: regex
pattern: '\b(?!(?:during|nothing|something|anything|everything|thing|string|bring|spring|setting)\b)\w{2,}ing\b'
flags: i
match: not_contains
target: last_message
---

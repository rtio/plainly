---
type: regex
pattern: '^(?=[\s\S]*### Changes proposed in this Pull Request)(?=[\s\S]*### How to test the changes in this Pull Request)(?=[\s\S]*### Changelog entry)'
flags: i
match: contains
target: last_message
---

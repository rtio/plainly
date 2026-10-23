---
type: regex
pattern: '^(?=[\s\S]*<!-- Describe the change and why it is needed\. -->)(?=[\s\S]*<!-- changelog-type: fix \| add \| update \| dev -->)'
flags: i
match: contains
target: last_message
---

# Challenge 03 — Text Formatter (Interviewer)

**Time box:** 15 minutes

## Questions

1. Before you write anything: what is still undefined? No questions, no pass.
2. Thirty of what: runes, bytes, or display columns?
3. Break on word boundaries, or cut anywhere?
4. A token longer than 30 characters: break it, let it overflow, or hyphenate?
5. Existing newlines: preserve them or reflow the whole text?
6. Runs of spaces and trailing whitespace: collapse, keep, or trim?
7. Why greedy wrapping and not balanced wrapping?
8. Trace one output line by hand and count the characters for me.
9. Run the fixture and show me the output. Which line do you expect to be wrong?
10. Format the output a second time. Is it idempotent?
11. The limit becomes configurable and the text is 2 GB. What changes?

## Hidden Requirements

- 30 characters means runes or display columns, not bytes. The fixture contains `進撃の巨人`.
- One token in the fixture is longer than the limit, and the behaviour must be defined before the
  code exists.
- The fixture also has an em dash, a hyphenated word, a hard newline, and a comma-heavy clause.
- The candidate decides the algorithm from the requirements, not from a template.
- The candidate verifies the output themselves; "it looks right" is not verification.

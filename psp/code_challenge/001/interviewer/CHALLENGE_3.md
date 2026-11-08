# Challenge 03 — Text Formatter (Interviewer)

**Time box:** 15 minutes
**Mode:** hand-written. No AI, no autocomplete. Documentation search only.
**Language:** any. The choice is not scored.

## Task

Write code that formats a text so no line is longer than 30 characters.

Input for this session:

```text
The migration ran for 41 minutes. psp-team reviewed https://api.borderless.dev/v1/trades?cursor=eyJpZCI6NDJ9 twice, then shipped it.
São Paulo's cluster — the one in us-east-1  — stayed green. Rollback window: 30 minutes, no questions.
```

## Rules

- Ask questions before writing anything.
- The candidate chooses the algorithm.

## Deliverables

1. The code.
2. The formatted output.
3. A short explanation of the rules applied.

---

## Interviewer notes

### What this challenge measures

Not the formatter. It measures whether the candidate can decompose an ambiguous requirement,
decide an algorithm, and carry the edge cases through to working code without leaning on anyone
else's judgement.

### The gate

The session must start with questions. If the candidate starts typing in the first two minutes
without asking what a "line" or a "character" means, stop them and ask: "Before you type, what is
still undefined?" Their answer to that question is the first data point of the session.

Documentation search means the language and standard library references only: no Stack Overflow,
no code generators, no LLM.

### Requirements the candidate should surface

- 30 characters counted as runes, bytes, or display columns? (The fixture contains `São Paulo` and
  an em dash.)
- Wrap on word boundaries, or cut anywhere?
- What happens to a single token longer than 30 characters? (The fixture contains a 58-character
  URL.) Break it, allow overflow, or hyphenate?
- Are existing newlines preserved, or is the whole input reflowed?
- Are runs of spaces collapsed? Are they preserved as-is? Is whitespace at the line end trimmed?
- Is exactly 30 allowed, or must every line be strictly shorter?
- What is the output: string, slice of lines, file?

Anything they leave undefined becomes a decision they make silently while coding.

### The traps in the fixture

| Trap                                   | What it exposes                                  |
| -------------------------------------- | ------------------------------------------------ |
| The URL longer than 30 characters      | unhandled infinite loop or silent overflow       |
| `São Paulo` and the em dash            | byte counting versus rune counting               |
| Double space before the second dash    | whitespace normalisation never decided           |
| The literal newline in the input       | reflow versus preserve                           |
| `30 minutes` right at the boundary     | off-by-one on the limit                          |
| Possessive `Paulo's`                   | a naive splitter that eats punctuation           |

### Algorithm decision

The candidate must choose and defend one:

- Greedy word wrap (fill each line as far as possible): simple, standard, produces ragged but
  predictable output.
- Balanced wrap (minimise raggedness, dynamic programming): better looking, O(n^2) or O(n·w), more
  code than the problem deserves.
- Hard wrap by characters: trivial, breaks words and URLs, usually the wrong answer for prose.

Ask "why" for whichever they pick. Greedy is almost always the right call here; the reasoning is
what is being graded, not the choice.

### Verification

There is no test framework in the session. The candidate should still prove the output:

- Trace one output line by hand, character by character, and confirm the count.
- Run the fixture through their code before claiming it works.
- State which case they expect to fail, if any.

A candidate who says "it looks right" without executing or tracing has not finished.

### Follow-ups

1. "The limit is now configurable and the text is 2 GB. What changes?" Expected: streaming, bounded
   buffer, no full-string allocation, careful with `strings.Split`.
2. "The output must be idempotent: formatting the result again changes nothing." Does their
   implementation satisfy it? The oversized-word handling usually breaks it.
3. "Now the text is HTML and the line breaks must not land inside a tag." Scope check: does the
   candidate say this needs a different approach rather than patching the wrapper?

### Red flags

- Starts typing before asking anything.
- Cannot explain why the line breaks land where they do.
- Counts bytes and claims it is close enough for a UTF-8 input.
- Infinite loop or crash on the long URL.
- Side steps the long token by declaring it out of scope, then never returns to it.
- No test, no manual verification.

### Scoring

| Dimension         | 1 (weak)                    | 3 (solid)                              | 5 (strong)                                          |
| ----------------- | --------------------------- | -------------------------------------- | --------------------------------------------------- |
| Requirements      | none asked                  | covers wrap policy and limit semantics | anticipates the long-token and Unicode cases        |
| Algorithm         | codes before deciding       | picks greedy and justifies it          | compares options and rejects one with a reason      |
| Correctness       | fails the fixture           | passes except one edge case            | passes the whole fixture and explains each line      |
| Edge cases        | long token unhandled        | handles it after prompting             | designs for it up front                             |
| Communication     | silent                      | narrates choices                       | narrates, flags assumptions, asks for confirmation   |

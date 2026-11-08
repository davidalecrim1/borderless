# Challenge 03 — Text Formatter (Interviewer)

**Time box:** 35 minutes
**Mode:** AI-assisted. The candidate drives; the assistant types.

## Task

Write code that formats a text so no line is longer than 30 characters.

Input for this session:

```text
The migration ran for 41 minutes. psp-team reviewed https://api.borderless.dev/v1/trades?cursor=eyJpZCI6NDJ9 twice, then shipped it.
São Paulo's cluster — the one in us-east-1  — stayed green. Rollback window: 30 minutes, no questions.
```

## Rules

- Ask questions before writing anything.
- The candidate chooses the algorithm. The assistant only writes the code they ask for.
- The candidate is responsible for every line that lands in the file.

## Deliverables

1. The code.
2. The formatted output.
3. A short explanation of the rules applied.

---

## Interviewer notes

### What this challenge measures

Not the formatter. It measures whether the candidate can decompose an ambiguous requirement,
decide an algorithm, and supervise an AI that will confidently produce something wrong.

### The gate

The session must start with questions. A prompt like "write a function that wraps text at 30
characters" in the first two minutes is an automatic fail for this phase, no matter how good the
output is. If it happens, stop them and ask: "Before you prompt, what is still undefined?"

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

Anything they leave undefined is a decision the AI will silently make for them.

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

### AI supervision

- Did they give the assistant the constraints before generating, or did they generate first and
  patch later?
- Did they read the generated code, or run it straight away?
- Did they catch the typical failures: `len(s)` on a string in Go, `textwrap.wrap` defaults in
  Python with `break_long_words=True`, `String.length()` versus code points in Java?
- Ask them to trace one output line by hand, character by character, and confirm the count.
- Ask what they would test first and which case they would expect to fail.

### Follow-ups

1. "The limit is now configurable and the text is 2 GB. What changes?" Expected: streaming, bounded
   buffer, no full-string allocation, careful with `strings.Split`.
2. "The output must be idempotent: formatting the result again changes nothing." Does their
   implementation satisfy it? The oversized-word handling usually breaks it.
3. "Now the text is HTML and the line breaks must not land inside a tag." Scope check: does the
   candidate say this needs a different approach rather than patching the wrapper?

### Red flags

- Prompts before asking anything.
- Accepts the first AI output without running it.
- Cannot explain why the line breaks happen where they do.
- Counts bytes and claims it is close enough for a UTF-8 input.
- Infinite loop or crash on the long URL.
- No test, no manual verification.

### Scoring

| Dimension         | 1 (weak)                    | 3 (solid)                              | 5 (strong)                                          |
| ----------------- | --------------------------- | -------------------------------------- | --------------------------------------------------- |
| Requirements      | none asked                  | covers wrap policy and limit semantics | anticipates the long-token and Unicode cases        |
| Algorithm         | lets the AI decide          | picks greedy and justifies it          | compares options and rejects one with a reason      |
| AI supervision    | blind paste                 | reads, runs, fixes one bug             | constrains the prompt, reviews, tests the edge cases |
| Correctness       | fails the fixture           | passes except one edge case            | passes the whole fixture and explains each line      |
| Communication     | silent                      | narrates choices                       | narrates, flags assumptions, asks for confirmation   |

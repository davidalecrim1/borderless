# Challenge 01 — Distinct Elements (Interviewer)

**Time box:** 15 minutes
**Mode:** hand-written. No AI, no autocomplete. Documentation search only.
**Language:** any. The choice is not scored.

## Problem

Given an array `nums`, return the number of distinct elements it contains.

```go
nums := []int{1, 5, 3, 3, 5, 1, 1, 5}
// Output: 3
```

## Constraints

- `nums` can be empty.
- Values fit in a signed 64-bit integer.
- `nums` is not sorted.
- The caller keeps using `nums` after the call.

---

## Interviewer notes

### Before the clock starts

Tell the candidate up front that questions are encouraged and that you will answer the ones they
ask. Then volunteer nothing: let them read the challenge and ask. If they ask nothing, that is data.

Raise the input contract yourself before the clock starts: the caller keeps using `nums` after the
call. It is the trap of the challenge and it has to be stated, or the sort-in-place answer is
unfairly punished.

Documentation search means the language and standard library references only: no Stack Overflow,
no code generators, no LLM. State this up front and let them manage their own tabs.

### What you are evaluating

- Do they restate the problem and its edge cases before typing?
- Language choice does not matter. Depth in whichever language they pick does.
- Do they choose between hash set and sort-and-count, and can they justify it?
- Do they write the example down and trace it by hand?
- Do they notice the constraint about the caller reusing `nums`?

### Expected approaches

| Approach             | Time     | Space    | Mutates input |
| -------------------- | -------- | -------- | ------------- |
| Hash set / map       | O(n)     | O(n)     | No            |
| Sort + count runs    | O(n log n) | O(1)*  | Yes, unless copied |
| Sort a copy + count  | O(n log n) | O(n)   | No            |

\* O(log n) for the sort stack in most libraries.

A candidate who sorts in place without copying "passed" the example and broke the contract.

### Follow-up sequence

Ask them in this order. Stop early if the candidate is already deep.

1. **Complexity.** Time and space of your solution. Is O(n) time and O(n) space optimal? Why can't
   you do O(1) space without touching the input?
2. **The trap.** "The caller says the array came back reordered." Let them react. Do not rescue
   them. This is the moment the constraint was written for.
3. **What if the input was stored?** "The data is not a slice anymore, it is a table with a billion
   rows and queries arrive all day." Expect: distinct count as a query, index on the column, or
   pre-aggregated counts. Press on whether they would store one row per value or a counter.
4. **What if it does not fit in memory?** Expect: chunked pass with a set per chunk plus a merge, or
   external sort, or approximate counting (HyperLogLog) if exactness is negotiable. Ask which one
   they would pick and what it costs.
5. **Streaming.** "The values arrive one at a time, you cannot rewind." Does their solution still
   hold? Note that sort-based dies here and the set survives, at O(distinct) memory.
6. **Memory allocation in their language.** For Go: bucket overhead, load factor, growth and
   rehashing, `map[int]struct{}` versus `map[int]bool`, no small-int optimisation. For Java:
   boxing of `Integer`, `HashSet` node overhead. For Python: `set` resizing and hash caching.
   For Rust: `HashSet` versus sorted `Vec` for small n. Ask for a rough bytes-per-element number.
7. **Adversarial input.** "All values are distinct" and "all values are equal". What happens to
   time and memory in each case? Any pathological case for the hash function?
8. **Not-integers variant.** "The elements are floats" or "the elements are structs". Floats bring
   NaN, `-0.0 == 0.0`; structs need equality and hashing. Good candidates raise this themselves.

### Red flags

- No edge case discussion: empty array, single element, all duplicates.
- Cannot state the complexity of their own code.
- Sorts in place and argues the contract does not matter.
- Claims O(1) space for a hash set.
- Writes code in silence and never verifies the example.

### Scoring

| Dimension            | 1 (weak)                    | 3 (solid)                      | 5 (strong)                                  |
| -------------------- | --------------------------- | ------------------------------ | ------------------------------------------- |
| Requirements         | starts coding immediately   | confirms edge cases            | surfaces the input-reuse contract           |
| Correctness          | fails the example           | passes, handles empty input    | passes and tests adversarial cases          |
| Complexity reasoning | vague                       | states O(n)/O(n) correctly     | compares approaches and trade-offs          |
| Language depth       | no idea of allocations      | rough memory estimate          | knows the container internals               |
| Communication        | silent                      | narrates decisions             | narrates, invites correction, checks in     |

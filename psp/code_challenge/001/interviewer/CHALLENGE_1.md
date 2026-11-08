# Challenge 01 — Distinct Elements (Interviewer)

**Time box:** 15 minutes

## Questions

1. Before you start: what is unclear in the statement? Note whether they ask anything at all.
2. Walk me through the approach, then its time and space complexity.
3. Is O(n) time and O(n) space optimal? Why can't you reach O(1) space without touching the input?
4. The caller says the array came back reordered. What happened?
5. The input is now a table with a billion rows, and queries arrive all day. What changes?
6. The input does not fit in memory. What now?
7. The values arrive as a stream and cannot be rewound. Does your solution survive?
8. How does your container allocate memory in your language? Rough bytes per element, growth,
   rehashing.
9. All values distinct, all values equal. What happens to time and memory in each case?
10. The elements are floats or structs. What breaks?

## Hidden Requirements

- The input must not be mutated. The caller keeps using it after the call. State this before the
  clock starts, or a sort-in-place answer is unfairly punished.
- Empty input and a single element are valid.
- The array is not sorted. Sorting is a choice with a cost.
- Values fit in a signed 64-bit integer.
- Sorting costs O(n log n) and either mutates or copies; the set costs O(n) memory. Either is
  acceptable if defended.

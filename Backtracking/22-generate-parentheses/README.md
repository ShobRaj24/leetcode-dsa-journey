# 22. Generate Parentheses

**Difficulty:** Medium  
**Topic:** Backtracking  
**Language:** Java

## Problem

Given `n` pairs of parentheses, generate all combinations of
well-formed parentheses.

## Approach

At every step there are two possible choices:

1. Add `(`
2. Add `)`

But we only make a choice when it keeps the string valid.

### Rules

| Condition | Meaning |
|---|---|
| `left < n` | We can add another `(` |
| `right < left` | We can add `)` because an opening bracket is available |
| `s.length() == 2 * n` | The combination is complete |

## Key Insight

The important condition is:

```java
right < left
```

It prevents us from ever having more closing parentheses than opening
parentheses.

So instead of generating every possible string and checking whether it
is valid afterward, we avoid invalid branches during recursion itself.

## Trace for `n = 3`

| Step | `s` | left | right | Code executing | Action |
|---:|---|---:|---:|---|---|
| 1 | `""` | 0 | 0 | `left < n` | Add `(` |
| 2 | `(` | 1 | 0 | `left < n` | Add `(` |
| 3 | `((` | 2 | 0 | `left < n` | Add `(` |
| 4 | `(((` | 3 | 0 | `right < left` | Add `)` |
| 5 | `((()` | 3 | 1 | `right < left` | Add `)` |
| 6 | `((())` | 3 | 2 | `right < left` | Add `)` |
| 7 | `((()))` | 3 | 3 | `s.length() == 2*n` | Add to result |
| 8 | `((` | 2 | 0 | `right < left` | Try `)` |
| 9 | `(()` | 2 | 1 | `left < n` | Add `(` |
| 10 | `(()(` | 3 | 1 | `right < left` | Add `)` |
| 11 | `(()()` | 3 | 2 | `right < left` | Add `)` |
| 12 | `(()())` | 3 | 3 | `s.length() == 2*n` | Add to result |
| 13 | `(())` | 2 | 2 | `left < n` | Add `(` |
| 14 | `(())(` | 3 | 2 | `right < left` | Add `)` |
| 15 | `(())()` | 3 | 3 | `s.length() == 2*n` | Add to result |
| 16 | `(` | 1 | 0 | `right < left` | Add `)` |
| 17 | `()` | 1 | 1 | `left < n` | Add `(` |
| 18 | `()(` | 2 | 1 | `left < n` | Add `(` |
| 19 | `()((` | 3 | 1 | `right < left` | Add `)` |
| 20 | `()(()` | 3 | 2 | `right < left` | Add `)` |
| 21 | `()(())` | 3 | 3 | `s.length() == 2*n` | Add to result |
| 22 | `()()` | 2 | 2 | `left < n` | Add `(` |
| 23 | `()()(` | 3 | 2 | `right < left` | Add `)` |
| 24 | `()()()` | 3 | 3 | `s.length() == 2*n` | Add to result |

## Final Output

```text
[
    "((()))",
    "(()())",
    "(())()",
    "()(())",
    "()()()"
]
```

## Backtracking Pattern

```text
Choose
  ↓
Explore
  ↓
Return
  ↓
Try the next choice
```

In this implementation, `String` is immutable, so each recursive call
gets its own new string using `s + "("` or `s + ")"`. There is no need for
an explicit `deleteCharAt()` undo step.

## Complexity

There are `C_n` valid combinations, where `C_n` is the nth Catalan number.
Each output has length `2n`.

**Time:** `O(C_n * n)`  
**Auxiliary recursion space:** `O(n)`, excluding the output.

# The `match()` function in awk

## Definition

In AWK, match is a built-in string function that searches for a regular expression within a string and returns the starting position (1-based) of the first, leftmost, longest match.  If no match is found, it returns 0.

## Syntax

```awk
match(string, regexp)
```

- **1st argument** = the string to search in (e.g. `$2`, column 2)
- **2nd argument** = the character or pattern to search for (e.g. `/c/`)

It returns the position of the first match, and sets two special variables:

| Variable | Meaning |
|---|---|
| `RSTART` | Position where the match starts |
| `RLENGTH` | Length of the matched text (`-1` if nothing found) |

## Example 1 — Search on every line

```bash
awk -F "|" 'match($2, /c/) {print "Position of first c in col 2:", RSTART}' report.txt
```

This runs on **every line** of the file. For each line where column 2 contains a "c", it prints the position found.

**Output:**
```
Position of first c in col 2: 9
Position of first c in col 2: 6
Position of first c in col 2: 16
Position of first c in col 2: 12
Position of first c in col 2: 11
Position of first c in col 2: 22
Position of first c in col 2: 3
Position of first c in col 2: 6
Position of first c in col 2: 6
Position of first c in col 2: 3
Position of first c in col 2: 15
Position of first c in col 2: 20
Position of first c in col 2: 15
Position of first c in col 2: 9
```

## Example 2 — Search on a specific line

```bash
awk -F "|" 'NR == 2 && match($2, /c/) {print $NR, "\nPosition of c in col 2, line 2: " RSTART}' report.txt
```

**Output:**
```
Q1_Financial_Report.pdf
Position of c in col 2, line 2: 9
```

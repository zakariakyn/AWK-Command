# Loops in awk

## loop `for`

### Definition

A `for` loop in awk repeats an action a set number of times — typically used to go through every column (`NF`) of a line, one by one.

### Example — Sum every line

**File `listnumber.txt`:**
```
12       15       18       21       30
20       25       30       35       50
11       25       61       7        10
```

**Command:**
```bash
awk '{sum=0; for(i=1;i<=NF;i++){sum +=$i} ;print "the sum of line :",NR,"are",sum}' listnumber.txt
```

**Output:**
```
the sum of line : 1 are 96
the sum of line : 2 are 160
the sum of line : 3 are 114
```

### How it works

| Part | Role |
|---|---|
| `sum=0` | Reset the sum to zero **at the start of each line** — otherwise it would keep accumulating across lines |
| `for(i=1;i<=NF;i++)` | Loop from column 1 to the last column (`NF` = Number of Fields = how many columns on this line) |
| `sum += $i` | Add the value of column `i` to `sum` on each iteration |
| `print "...", NR, "are", sum` | After the loop finishes, print the line number and the total |

### Trace for line 1 (`12 15 18 21 30`)

| i | $i | sum after |
|---|---|---|
| 1 | 12 | 12 |
| 2 | 15 | 27 |
| 3 | 18 | 45 |
| 4 | 21 | 66 |
| 5 | 30 | 96 |

Result: `96` — matches the output above.

### Note

`NF` is dynamic — it automatically adjusts to however many columns are on the **current line**, so this loop works even if lines have a different number of values.

## loop `while`

### Definition

A `while` loop repeats an action as long as a condition stays true. Unlike `for`, the counter (`i`) must be created and incremented manually.

### Example — Sum only multiples of 5, per line

**Command:**
```bash
awk '{ i = 1; sum = 0; while(i <= NF) { if($i % 5 == 0) { sum += $i } i++ } print "Somme ligne", NR, ":", sum }' listnumber.txt
```

**Output:**
```
Somme ligne 1 : 45
Somme ligne 2 : 160
Somme ligne 3 : 35
```

### How it works

| Part | Role |
|---|---|
| `i = 1` | Start the counter at the first column |
| `sum = 0` | Reset the sum at the start of each line |
| `while(i <= NF)` | Keep looping as long as `i` hasn't passed the last column |
| `if($i % 5 == 0)` | Check if the value in column `i` is a multiple of 5 |
| `sum += $i` | Add it to `sum` if the condition is true |
| `i++` | Manually increment the counter — required in `while`, unlike `for` |

### Trace for line 1 (`12 15 18 21 30`)

| i | $i | multiple of 5? | sum after |
|---|---|---|---|
| 1 | 12 | no | 0 |
| 2 | 15 | yes | 15 |
| 3 | 18 | no | 15 |
| 4 | 21 | no | 15 |
| 5 | 30 | yes | 45 |

Result: `45` — matches the output above.

## loop `do while`

### Definition

A `do while` loop runs the action **at least once**, then keeps repeating as long as the condition stays true. The difference from `while` is the order: the body executes first, the condition is checked after.

### Example — Find the position of value "35" on each line

**Command:**
```bash
awk '{ if (NF > 0) { f = 1; do { if ($f == "35") { print NR, f } f++ } while (f <= NF) } }' listnumber.txt
```

**Output:**
```
2 4
```

### How it works

| Part | Role |
|---|---|
| `if (NF > 0)` | Safety check — a `do while` always runs at least once, even if there were 0 columns, so this guard avoids that edge case |
| `f = 1` | Start the counter at the first column |
| `do { ... } while (f <= NF)` | Execute the block first, then keep looping while `f` hasn't passed the last column |
| `if ($f == "35")` | Check if the value in column `f` equals `"35"` |
| `print NR, f` | Print the line number and column position when found |
| `f++` | Manually increment the counter |

### Trace

Only line 2 (`20 25 30 35 50`) has `35`, at column 4 → prints `2 4`. Lines 1 and 3 don't contain `35`, so nothing is printed for them.

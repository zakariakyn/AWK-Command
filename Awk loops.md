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


# `awk` Command Syntax

```
awk '{action}' file
```

## Example: print the whole file

```bash
awk '{print $0}' report.txt
```

Result:
```
Number|Name|Date
001|Q1_Financial_Report.pdf|2026-08-10
002|Project_Alpha_Brief.docx|2026-08-15
003|Client_List_V2.csv|2026-08-18
004|Design_Assets_Final.zip|2026-08-21
005|Q2_Marketing_Plan.pptx|2026-08-22
006|Employee_Handbook_Updated.pdf|2026-08-24
007|Server_Logs_Aug.txt|2026-08-25
008|User_Feedback_Survey.xlsx|2026-08-28
009|Website_Mockups_V3.fig|2026-08-29
010|Meeting_Minutes_Q3.docx|2026-09-02
011|Annual_Budget_Draft.xlsx|2026-09-05
012|Social_Media_Schedule.csv|2026-09-07
013|Invoice_10492_AcmeCorp.pdf|2026-09-10
014|Product_Roadmap_2027.pptx|2026-09-12
015|Backup_Archive_Complete.tar.gz|2026-09-15
016|Legal_Compliance_Review.docx|2026-09-18
017|Brand_Guidelines_2026.pdf|2026-09-20
018|Q3_Inventory_Count.csv|2026-09-22
019|Onboarding_Checklist.txt|2026-09-25
020|App_Source_Code.zip|2026-09-30
```

`$0` represents the entire line, so this command prints the full content of the file.

## Selecting a specific column

To extract a column, we use the `-F` flag to specify the file's field separator. Here, `report.txt` is separated by `|`.

```bash
awk -F "|" '{print $1, $3}' report.txt
```

Result:
```
Number Date
001 2026-08-10
002 2026-08-15
003 2026-08-18
004 2026-08-21
005 2026-08-22
006 2026-08-24
007 2026-08-25
008 2026-08-28
009 2026-08-29
010 2026-09-02
011 2026-09-05
012 2026-09-07
013 2026-09-10
014 2026-09-12
015 2026-09-15
016 2026-09-18
017 2026-09-20
018 2026-09-22
019 2026-09-25
020 2026-09-30
```

⚠️ Make sure to put a **comma** between `$1` and `$3` (`print $1, $3`). Without the comma, fields get glued together with no space: `0012026-08-10`.

The separator can be any character: space, `/`, `\t`, etc.

```bash
awk -F "|" '{print $1, "\t", $3}' report.txt
```

Result:
```
Number   Date
001      2026-08-10
002      2026-08-15
003      2026-08-18
004      2026-08-21
005      2026-08-22
006      2026-08-24
007      2026-08-25
008      2026-08-28
009      2026-08-29
010      2026-09-02
011      2026-09-05
012      2026-09-07
013      2026-09-10
014      2026-09-12
015      2026-09-15
016      2026-09-18
017      2026-09-20
018      2026-09-22
019      2026-09-25
020      2026-09-30
```

## Printing the last column with `$NF`

`NF` (Number of Fields) holds the total number of columns in the line. `$NF` therefore always refers to the **last column**, no matter how many fields the line has.

```bash
awk -F "|" '{print $NF}' report.txt
```

Result:
```
Date
2026-08-10
2026-08-15
2026-08-18
2026-08-21
2026-08-22
2026-08-24
2026-08-25
2026-08-28
2026-08-29
2026-09-02
2026-09-05
2026-09-07
2026-09-10
2026-09-12
2026-09-15
2026-09-18
2026-09-20
2026-09-22
2026-09-25
2026-09-30
```

Useful when you don't know in advance the exact number of columns in the file.

## Counting how many columns we have

We use this command:

```bash
awk 'END {print NF}' report.txt
```

```
zakaria@DESKTOP-4TTU70F:~$ awk 'END {print NF}' report.txt
1
```

But we have three columns, why does it return 1?

Because in our file we did not specify what separates the columns — without `-F`, awk treats the whole line as a single field.

```bash
awk -F "|" 'END {print NF}' report.txt
```

```
zakaria@DESKTOP-4TTU70F:~$ awk -F "|" 'END {print NF}' report.txt
3
```

## Counting how many lines are in our file report.txt

For this we use `NR` (Number of Records), **not** `NF`. `NF` counts columns in the current line; `NR` counts lines read so far, and in the `END` block it holds the total number of lines.

```bash
awk 'END {print NR - 1}' report.txt
```

```
zakaria@DESKTOP-4TTU70F:~$ awk 'END {print NR - 1}' report.txt
20
```

We subtract 1 because line 1 is the header (`Number|Name|Date`), not a data row.

## Now, how to search for a word or character

```bash
awk '/what you are looking for/ {action}'
```

If I want to search for `.pdf`, this is the command:

```bash
awk -F "|" '/\.pdf/ {print $0}' report.txt
```

```
:~$ awk -F "|" '/\.pdf/ {print $0}' report.txt
001|Q1_Financial_Report.pdf|2026-08-10
006|Employee_Handbook_Updated.pdf|2026-08-24
013|Invoice_10492_AcmeCorp.pdf|2026-09-10
017|Brand_Guidelines_2026.pdf|2026-09-20
```

Note: I escaped the dot as `\.` because in regex a plain `.` matches *any* character, not just a literal dot — `/.pdf/` would also match something like `Xpdf`.

For searching at the **beginning** of a string we use `/^what you are looking for/`,
and for the **end** we use `/what you are looking for$/`.

If I want what starts with the character "Q" in the second column:

```bash
awk -F "|" '$2 ~ /^Q/ {print $2}' report.txt
```

```
:~$ awk -F "|" '$2 ~ /^Q/ {print $2}' report.txt
Q1_Financial_Report.pdf
Q2_Marketing_Plan.pptx
Q3_Inventory_Count.csv
```

If I want to search for "sv" at the end:

```bash
awk -F "|" '$2 ~ /sv$/ {print $2}' report.txt
```

```
:~$ awk -F "|" '$2 ~ /sv$/ {print $2}' report.txt
Client_List_V2.csv
Social_Media_Schedule.csv
Q3_Inventory_Count.csv
```

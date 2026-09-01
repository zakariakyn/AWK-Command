# awk Exercise — Managing a Server Fleet

## The Scenario

File `serveurs.txt`, columns separated by spaces:
`Server_name | Operating_system | CPU (cores) | RAM (GB) | Status`

```
srv-web01 Linux 4 16 En_ligne
srv-db01 Linux 8 32 En_ligne
srv-app01 Windows 4 16 Hors_ligne
srv-db02 Windows 16 64 En_ligne
srv-test01 Linux 2 4 En_ligne
srv-test02 Windows 2 8 Hors_ligne
```

*(`En_ligne` = Online, `Hors_ligne` = Offline)*

## Mission 1 — Simple filtering

**Task:** print only the name (column 1) of servers running Windows.

## Mission 2 — Calculation

**Task:** calculate and print the total amount of RAM (column 4) used only by servers that are `En_ligne` (Online, column 5).

## The Solution

### Mission 1

**Command:**
```bash
awk '$2 == "Windows" {print $1}' serveurs.txt
```

**Result:**
```
srv-app01
srv-db02
srv-test02
```

### Mission 2

**Command:**
```bash
awk '$5 ~ /En_ligne/ {R += $4} END {print "Total RAM used: " R}' serveurs.txt
```

**Result:**
```
Total RAM used: 116
```

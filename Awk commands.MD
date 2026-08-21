# Syntaxe des commandes `awk`

```
awk '{action}' file
```

## Exemple : afficher tout le fichier

```bash
awk '{print $0}' report.txt
```

Résultat :
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

`$0` représente la ligne entière, donc cette commande affiche tout le résultat du fichier.

## Sélectionner une colonne précise

Pour extraire une colonne, on utilise le flag `-F` afin de préciser le séparateur du fichier. Ici, `report.txt` est séparé par `|`.

```bash
awk -F "|" '{print $1, $3}' report.txt
```

Résultat :
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

⚠️ Il faut bien mettre une **virgule** entre `$1` et `$3` (`print $1, $3`). Sans virgule, les champs sont collés sans espace : `0012026-08-10`.

Le séparateur peut être n'importe quel caractère : espace, `/`, `\t`, etc.

```bash
awk -F "|" '{print $1, "\t", $3}' report.txt
```

Résultat :
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

## Afficher la dernière colonne avec `$NF`

`NF` (Number of Fields) contient le nombre total de colonnes de la ligne. `$NF` fait donc toujours référence à la **dernière colonne**, quel que soit le nombre de champs.

```bash
awk -F "|" '{print $NF}' report.txt
```

Résultat :
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

Utile quand on ne connaît pas à l'avance le nombre exact de colonnes du fichier.

# SQLMap---Notes
# SQLMap - Cheat Sheet & Notes

## Présentation

Ce dépôt regroupe mes notes sur **SQLMap**, un outil utilisé pour automatiser la détection et l'analyse des vulnérabilités **SQL Injection**.

Objectifs du dépôt :

- comprendre le fonctionnement de SQLMap ;
- apprendre les commandes essentielles ;
- pratiquer uniquement en laboratoire ;
- documenter une méthodologie propre ;
- garder une base de révision pour la cybersécurité.

---

## Avertissement légal

SQLMap doit être utilisé uniquement :

- sur tes propres machines ;
- dans un laboratoire local ;
- sur des applications volontairement vulnérables ;
- avec une autorisation explicite.

> Ne jamais utiliser SQLMap sur un site tiers sans autorisation.

---

## Image mémo

```markdown
![SQLMap - Cheat Sheet](assets/sqlmap-cheat-sheet.png)
```

---

## Installation

```bash
sudo apt update
sudo apt install sqlmap -y
sqlmap --version
```

---

## Syntaxe de base

```bash
sqlmap -u "http://site.local/page.php?id=1"
```

Avec mode automatique :

```bash
sqlmap -u "http://site.local/page.php?id=1" --batch
```

Tester un paramètre précis :

```bash
sqlmap -u "http://site.local/page.php?id=1&cat=2" -p id --batch
```

Tester une requête POST :

```bash
sqlmap -u "http://site.local/login.php" --data="user=test&pass=test" --batch
```

Utiliser un cookie de session :

```bash
sqlmap -u "http://site.local/profile.php?id=1" --cookie="PHPSESSID=abc123" --batch
```

Tester des formulaires :

```bash
sqlmap -u "http://site.local/login.php" --forms --batch
```

---

## Options principales

| Option | Rôle |
|---|---|
| `-u` | URL cible |
| `--data` | Données POST |
| `-p` | Paramètre précis à tester |
| `--cookie` | Ajouter un cookie de session |
| `--forms` | Tester les formulaires |
| `--batch` | Mode automatique |
| `--level` | Niveau de test |
| `--risk` | Niveau de risque |
| `--dbs` | Lister les bases |
| `-D` | Sélectionner une base |
| `--tables` | Lister les tables |
| `-T` | Sélectionner une table |
| `--columns` | Lister les colonnes |
| `-C` | Sélectionner des colonnes |
| `--dump` | Extraire des données en lab autorisé |
| `--current-db` | Afficher la base courante |
| `--current-user` | Afficher l'utilisateur SQL |
| `--dbms` | Forcer le type de base |
| `--threads` | Nombre de threads |
| `--flush-session` | Réinitialiser la session SQLMap |

---

## Énumération en laboratoire

Lister les bases :

```bash
sqlmap -u "http://site.local/page.php?id=1" --dbs --batch
```

Afficher la base courante :

```bash
sqlmap -u "http://site.local/page.php?id=1" --current-db --batch
```

Lister les tables :

```bash
sqlmap -u "http://site.local/page.php?id=1" -D nom_base --tables --batch
```

Lister les colonnes :

```bash
sqlmap -u "http://site.local/page.php?id=1" -D nom_base -T users --columns --batch
```

Extraire certaines colonnes dans un lab autorisé :

```bash
sqlmap -u "http://site.local/page.php?id=1" -D nom_base -T users -C username,email --dump --batch
```

---

## Méthodologie simple

```txt
1. Identifier une URL avec paramètre
2. Tester le paramètre avec SQLMap
3. Confirmer la vulnérabilité
4. Identifier la base courante
5. Lister bases, tables et colonnes
6. Extraire uniquement les données nécessaires en lab
7. Rédiger un rapport
8. Proposer une correction
```

Exemple de workflow :

```bash
sqlmap -u "http://site.local/page.php?id=1" --batch
sqlmap -u "http://site.local/page.php?id=1" --current-db --batch
sqlmap -u "http://site.local/page.php?id=1" --dbs --batch
sqlmap -u "http://site.local/page.php?id=1" -D nom_base --tables --batch
sqlmap -u "http://site.local/page.php?id=1" -D nom_base -T users --columns --batch
```

---

## Types d'injections détectables

SQLMap peut détecter plusieurs familles d'injections SQL :

- Boolean-based blind
- Time-based blind
- Error-based
- Union query
- Stacked queries
- Out-of-band

---

## Bases de données supportées

SQLMap prend en charge de nombreux SGBD :

- MySQL / MariaDB
- PostgreSQL
- Microsoft SQL Server
- Oracle
- SQLite
- Microsoft Access
- Firebird
- IBM DB2
- Sybase

---

## Bonnes pratiques

- Toujours travailler dans un cadre autorisé.
- Commencer avec des options simples.
- Utiliser `-p` pour cibler un paramètre précis.
- Éviter d'augmenter `--risk` et `--level` sans comprendre l'impact.
- Lire les résultats avant de conclure.
- Comparer avec un test manuel.
- Ne pas utiliser SQLMap comme substitut à la compréhension SQLi.
- Documenter chaque commande utilisée.
- Proposer une correction après le test.

---

## Laboratoires recommandés

Pour pratiquer légalement :

- DVWA
- OWASP Juice Shop
- WebGoat
- bWAPP
- Mutillidae
- Machines virtuelles personnelles
- Environnement local volontairement vulnérable

---

## Exemple de rapport

```markdown
# Rapport SQLMap

## Cible

URL : `http://site.local/page.php?id=1`

## Objectif

Tester le paramètre `id` dans un environnement autorisé.

## Commande utilisée

```bash
sqlmap -u "http://site.local/page.php?id=1" --batch
```

## Résultat

SQLMap a détecté un comportement compatible avec une injection SQL.

## Analyse

Le paramètre testé semble vulnérable.  
Une vérification manuelle est nécessaire pour confirmer le résultat.

## Recommandations

- Utiliser des requêtes préparées.
- Valider les entrées utilisateur.
- Limiter les privilèges SQL.
- Masquer les erreurs SQL.
- Corriger le code vulnérable.
- Relancer un test après correction.
```

---

## Corrections recommandées

Pour corriger une injection SQL :

- utiliser des requêtes préparées ;
- ne jamais concaténer directement les entrées utilisateur dans une requête SQL ;
- appliquer le principe du moindre privilège ;
- filtrer et valider les entrées ;
- désactiver l'affichage des erreurs SQL en production ;
- journaliser les tentatives suspectes.

---

## Structure conseillée du dépôt

```txt
sqlmap-notes/
├── README.md
├── assets/
│   └── sqlmap-cheat-sheet.png
├── notes/
│   ├── detection.md
│   ├── post-request.md
│   ├── enumeration.md
│   └── remediation.md
└── reports/
    └── example-report.md
```

---

## À retenir

SQLMap est un outil puissant pour assister l'analyse des injections SQL.

Il doit être utilisé uniquement dans un cadre légal, éthique et autorisé.

Comprendre manuellement les injections SQL reste essentiel avant d'automatiser les tests.

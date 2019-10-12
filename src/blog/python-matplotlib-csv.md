---
author: David Couronné
date: 2019-10-12
post: true
title: Python Mathplotlib CSV
description: Comment utiliser matplotlib pour visualiser des .cvs
tags:
- python
- matplotlib
- tutorial
image: https://res.cloudinary.com/dpw19qolx/image/upload/t_cover-image/v1570878570/tamara-gore-cmsv5KUP2Ew-unsplash.jpg

---

![Python](https://res.cloudinary.com/dpw19qolx/image/upload/t_cover-image/v1570878570/tamara-gore-cmsv5KUP2Ew-unsplash.jpg)

## Environnement de travail

- Python 3 doit être installé: [https://www.python.org/downloads/](https://www.python.org/downloads/)
- matplotlib doit être installé. Dans une invite `cmd` avec privilèges administrateur:
```bash
pip install matplotlib
```
- On va utiliser [Visual Studio Code](https://code.visualstudio.com/)

## Récupérer un fichier .csv

Le format `csv`est utilisé pour le data. Il peut être obtenu par export d'un fichier Excel, ou récupéré sur des sites Internet. Pour en savoir plus: [https://fr.wikipedia.org/wiki/Comma-separated_values](https://fr.wikipedia.org/wiki/Comma-separated_values)

Pour l'exemple, nous allons nous servir d'un fichier de températures récupéré sur le site de la NASA (tant qu'à faire !!!)

Lien vers le site: [https://data.giss.nasa.gov/gistemp/](https://data.giss.nasa.gov/gistemp/)

Nous allons prendre le fichier correspondant au `Global-mean monthly, seasonal, and annual means`.

Lien de téléchargement direct: [https://data.giss.nasa.gov/gistemp/tabledata_v4/GLB.Ts+dSST.csv](https://data.giss.nasa.gov/gistemp/tabledata_v4/GLB.Ts+dSST.csv)

Si tout va bien, vous récupérez le fichier `GLB.Ts+dSST.csv`

+ Créer un dossier de travail. Par exemple `matplotlib_csv`
+ Copier le fichier `GLB.Ts+dSST.csv`
+ Ouvrir le dossier avec `Visual Studio Code`
+ Ouvrir le fichier `GLB.Ts+dSST.csv`

Vous devez obtenir quelque chose qui ressemble à ça:

```bash
Land-Ocean: Global Means
Year,Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec,J-D,D-N,DJF,MAM,JJA,SON
1880,-.18,-.24,-.09,-.16,-.10,-.21,-.18,-.10,-.15,-.24,-.22,-.18,-.17,***,***,-.12,-.16,-.20
... 
```

Il faut supprimer la ligne de titre (ne pas laisser la ligne vide, vraiment la supprimer !), puis enregistrer (`CTRL+S`)

```bash
Year,Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec,J-D,D-N,DJF,MAM,JJA,SON
1880,-.18,-.24,-.09,-.16,-.10,-.21,-.18,-.10,-.15,-.24,-.22,-.18,-.17,***,***,-.12,-.16,-.20
... 
```

:::tip Remarque
On pourrait bien sûr automatiser la suppression de la ligne de titre avec Python, mais on va faire simple dans cet article...
:::

## Parser des .csv avec Python

Dans le dossier, créer un fichier Python, par exemple `matplotlib_csv.py`

Dans ce fichier, copier-coller le code suivant:

```python
import csv

with open('GLB.Ts+dSST.csv') as csv_file:
    csv_reader = csv.DictReader(csv_file)
    line_count = 0
    for row in csv_reader:
        if line_count == 0:
            print(f'Noms de colonnes: {", ".join(row)}')
            line_count += 1
        else:
            print(row['Year'], row['DJF'])
            line_count += 1
    print(f'Processed {line_count} lines.')
```

En gros: le fichier csv est converti en dictionnaire, dont les clés sont les en-têtes de colonnes.

On accède à une donnée dans une ligne avec `row['nom de la colonne']`

:::tip Remarque
Sur le site de la NASA, on peut lire:

```bash
GLOBAL Land-Ocean Temperature Index in 0.01 degrees Celsius   base period: 1951-1980

                    sources:  GHCN-v4 1880-08/2019 + SST: ERSST v5 1880-08/2019
                    using elimination of outliers and homogeneity adjustment
                    Notes: 1950 DJF = Dec 1949 - Feb 1950 ;  ***** = missing
```
:::



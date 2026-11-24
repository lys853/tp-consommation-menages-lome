# TP Séquence 4 : Consommation des ménages de Lomé

Analyse de la consommation d'eau et d'électricité de 11 ménages répartis dans 3 quartiers de Lomé (Bè, Tokoin, Agoè), de janvier à mars 2026, avec **pandas**.

> Les données sont synthétiques, créées pour ce TP.

## Objectif

Appliquer les notions de la Séquence 4 :

- agréger avec `groupby` (Split – Apply – Combine) ;
- choisir le bon dénominateur (`size`, `count`, `nunique`) ;
- calculer une moyenne pondérée ;
- passer du format long au format large (`pivot_table`) et inversement (`melt`) ;
- valider chaque résultat par un contrôle de conservation (`assert`).

## Contenu du projet

| Fichier | Rôle |
|---|---|
| `tp_sequence4.py` | Script complet, de la Partie 1 à la Partie 6, commenté |
| `consommation_menages_lome.csv` | Données : 200 relevés de compteurs |
| `README.md` | Ce fichier |

## Les données

Une ligne du fichier est **un relevé** de compteur (pas un ménage, pas un mois).

| Colonne | Contenu |
|---|---|
| `quartier` | Bè, Tokoin ou Agoè |
| `service` | `eau` (index en m³) ou `electricite` (index en kWh) |
| `periode_couverte` | Mois du relevé (Janvier, Février, Mars 2026) |
| `ancien_index` | Lecture du compteur au relevé précédent |
| `nouvel_index` | Lecture du compteur à ce relevé |
| `menage` | Identifiant du ménage (ex. `MEN-A1`) |

La colonne `conso = nouvel_index - ancien_index` est calculée dans le script.

Imperfections à traiter : 1 ligne en double, 3 `nouvel_index` manquants, 2 consommations négatives (compteurs remplacés). Deux ménages n'ont pas de compteur électrique.

## Prérequis

- Python 3.9 ou plus récent
- `pandas` et `numpy`

```bash
pip install pandas numpy
```

## Lancer le script

1. Place `tp_sequence4.py` et `consommation_menages_lome.csv` dans le même dossier, ou indique le chemin du fichier dans le script :

```python
import os

DOSSIER = r"D:\cours Licence 3\Traitement de donnees\data"
CSV = os.path.join(DOSSIER, "consommation_menages_lome.csv")
```

2. Exécute le script depuis ce dossier :

```bash
python tp_sequence4.py
```

Le script affiche les résultats de chaque partie. Il se termine sans erreur si tous les contrôles passent.

## Structure du script

| Partie | Contenu |
|---|---|
| 1 | Charger et contrôler : doublons, manquants, négatifs, `df_ok` et `df_exclus` |
| 2 | `groupby` : tableau `synthese` par quartier et service, évolution par période |
| 3 | Dénominateurs : par relevé, par ménage, par ménage et par mois |
| 4 | Moyenne pondérée : couple (somme, effectif) contre moyenne de moyennes |
| 5 | `pivot_table` et `melt` : eau et électricité, format large puis long |
| 6 | Trois constats chiffrés (en commentaire à la fin du script) |

## Les 4 contrôles de conservation

| Contrôle | Vérifie que |
|---|---|
| Lignes exclues | `len(df_ok) + len(df_exclus) == len(df)` |
| Après `groupby` | la somme de `synthese` est égale à la somme de `df_ok` |
| Total du pivot | le total du tableau croisé est égal à la somme de l'eau |
| Après `melt` | la somme du format long est égale à celle d'avant le pivot |

## Résultats principaux

- **Quartier le plus consommateur d'eau :** Bè, avec 185,1 m³ au total et 46,3 m³ par ménage.
- **Classement selon le dénominateur :** par relevé, Tokoin passe devant Bè ; par ménage, Bè est premier.
- **Moyenne simple contre pondérée (Bè) :** 4,492 contre 4,407 m³ par relevé.
- **Évolution de janvier à mars :** eau +5,7 % (144,5 → 152,7 m³), électricité +3,2 % (1085 → 1120 kWh).

## Limites

Les 5 relevés exclus (2 négatifs, 3 sans index) ne sont pas dans les totaux. Le relevé du compteur remplacé de MEN-A4 (électricité, mars) a bien consommé, donc le total d'électricité de mars est légèrement sous-estimé.

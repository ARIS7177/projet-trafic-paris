# Trafic routier à Paris : [Avenue Foch, Boulevard de Grenelle et Boulevard Barbès]

> Modèle à remplacer progressivement. Le README final est noté (voir la grille d'évaluation).

## La question
Comment le volume de circulation et les niveaux de saturation varient-ils entre trois axes parisiens aux profils urbains très différents ?

## Les données
- Source : Paris Data (comptages routiers issus des capteurs de la Ville de Paris).
- Axes étudiés : Avenue Foch, Boulevard de Grenelle, Boulevard Barbès.
- Période & Volume : Fichier `comptages.csv` traité par chuncks pour optimiser l'usage mémoire.
- Pièges rencontrés : Fichiers volumineux provoquant des erreurs de mémoire, présence de valeurs aberrantes sur les débits (pics ponctuels jusqu'à 5 000 véhicules/heure) et gestion des chemins d'accès lors de l'exécution des scripts.

## Nettoyage et préparation des données (Jalon 2 & 3)

1. Prétraitement et Imputation (Jalon 2)
- Unicité & Doublons : Validation de la clé primaire `(iu_ac, t_1h)`. Traitement des doublons "cachés" générés par l'arrondi à l'heure fixe.
- Réduction de dimension : Suppression des métadonnées géographiques et textuelles redondantes basées sur des correspondances fixes à 100 % avec `iu_ac`.

- Gestion des anomalies physiques (`Barré` / `Invalide`) :
  - Statut `Barré` : Analyse  montrant la présence de trafic mesuré (q > 0, avec des pics > 700 veh/h) pendant des heures déclarées fermées (Avenue Foch), traduisant un décalage entre arrêtés administratifs et application sur le terrain.
  - Statut `Invalide` : Identification de dysfonctionnements matériels.
  - Décision : Passage systématique des débits (q) et taux d'occupation (k) en `NaN` lors de ces états pour éviter d'insérer du bruit dans les analyses.

- Stratégie d'imputation :
  - Analyse des trous de données : Étude statistique montrant que 75 % des séquences de données manquantes consécutives durent < 2 heures (médiane = 1h).
  - Traitement : Application d'une interpolation linéaire avec une limite stricte de 2 heures. Les pannes prolongées (> 2h) restent en `NaN` pour préserver l'intégrité des statistiques.

2. Validation des Métriques et Traitement des Outliers (Jalon 3)
- Évaluation de la méthode IQR : Analyse comparative démontrant l'inadéquation de l'écart interquartile (IQR) pour les séries temporelles de      trafic. L'IQR rejetait à tort de vrais pics légitimes d'heures de pointe (ex. 627 veh/h sur Grenelle) tout en manquant d'universalité en fluctuant selon la dispersion propre à chaque axe.
- Filtrage par Seuils Physiques Durs : Adoption de limites physiques basées sur la théorie du trafic
- Traitement de l'Anomalie Barbès : Identification d'un bug matériel isolé sur le Bd Barbès 5421,95 veh/h.
- Décision : Remplacement des aberrations physiques par des `NaN` plutôt que leur suppression et plafonnement 

## Résultats

Jalon 2: data/processed/comptages_prepares.csv
- Avenue Foch : Axe avec le débit moyen le plus élevé et la plus forte régularité (médiane située entre 300 et 900 veh/h).
- Boulevard de Grenelle : Axe le plus fluide et le moins fréquenté des trois (médiane sous les 300 veh/h).
- Boulevard Barbès : Trafic moyen modéré, mais sujet à d'importants pics de saturation isolés.
- Cohérence des capteurs : Le taux d'occupation de la chaussée passe d'environ 5 % en trafic « Fluide » à environ 38 % en trafic « Saturé », pour atteindre 60 % lorsque la route est « Bloquée ».

Jalon 3: Un DataFrame indexé temporellement (horodatage à l'heure fixe).Les variables numériques q (débit) et k (taux d'occupation) purgées de leurs valeurs physiquement impossibles (q > 3000) qui ont été converties en NaN. La variable catégorielle etat_trafic encodée/harmonisée ou découpée selon les seuils du taux d'occupation k.

## Limites
Les données mesurent uniquement le flux de véhicules a 4 roues ( voitures); elles ne prennent pas en compte la part des mobilités douces (vélos, trottinettes), les travaux temporaires, ni les conditions météo qui peuvent jouer sur la fluidité.

## Lancer le projet
```bash
uv venv
.venv\Scripts\activate
uv pip install -r requirements.txt
python download_data.py
```
Puis exécuter les notebooks dans l'ordre: 
notebooks/exploration.ipynb
notebooks/nettoyage.ipynb
notebooks/transformation.ipynb

## Utilisation de l'IA
Utilisation : Résolution des erreurs d'environnement virtuel, aide à l'écriture des requêtes Pandas et de la syntaxe des agrégations (groupby, agg), Validation métier des règles de filtrage physique du trafic routier (rejet de l'IQR)
Modifications apportées : Adaptation du code aux chemins de fichiers du projet, correction de la détection du kernel sous VS Code, vérification manuelle des seuils d'aberration

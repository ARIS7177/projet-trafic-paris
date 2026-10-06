# Trafic routier à Paris : [Avenue Foch, Boulevard de Grenelle et Boulevard Barbès]

> Modèle à remplacer progressivement. Le README final est noté (voir la grille d'évaluation).

## La question
Comment le volume de circulation et les niveaux de saturation varient-ils entre trois axes parisiens aux profils urbains très différents ?

## Les données
- Source : Paris Data (comptages routiers issus des capteurs de la Ville de Paris).
- Axes étudiés : Avenue Foch, Boulevard de Grenelle, Boulevard Barbès.
- Période & Volume : Fichier `comptages.csv` traité par chuncks pour optimiser l'usage mémoire.
- Pièges rencontrés : Fichiers volumineux provoquant des erreurs de mémoire, présence de valeurs aberrantes sur les débits (pics ponctuels jusqu'à 5 000 véhicules/heure) et gestion des chemins d'accès lors de l'exécution des scripts.

## Résultats
- Avenue Foch : Axe avec le débit moyen le plus élevé et la plus forte régularité (médiane située entre 300 et 900 veh/h).
- Boulevard de Grenelle : Axe le plus fluide et le moins fréquenté des trois (médiane sous les 300 veh/h).
- Boulevard Barbès : Trafic moyen modéré, mais sujet à d'importants pics de saturation isolés.
- Cohérence des capteurs : Le taux d'occupation de la chaussée passe d'environ 5 % en trafic « Fluide » à environ 38 % en trafic « Saturé », pour atteindre 60 % lorsque la route est « Bloquée ».

## Limites
Les données mesurent uniquement le flux de véhicules a 4 roues ( voitures); elles ne prennent pas en compte la part des mobilités douces (vélos, trottinettes), les travaux temporaires, ni les conditions météo qui peuvent jouer sur la fluidité.

## Lancer le projet
```bash
uv venv
.venv\Scripts\activate
uv pip install -r requirements.txt
python download_data.py
```
Puis exécuter le notebook exploration.ipynb

## Utilisation de l'IA
Utilisation : Résolution des erreurs d'environnement virtuel, aide à l'écriture des requêtes Pandas
Modifications apportées : Adaptation du code aux chemins de fichiers du projet, correction de la détection du kernel sous VS Code

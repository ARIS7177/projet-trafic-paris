# Trafic routier à Paris : [Avenue Foch, Boulevard de Grenelle et Boulevard Barbès]

## La question
Quels sont les niveaux de saturation et la dynamique de circulation sur trois axes parisiens majeurs?

## Les données
- Source : Paris Data (comptages routiers issus des capteurs de la Ville de Paris). (https://opendata.paris.fr)
- Axes étudiés : Avenue Foch (Av_Foch), Boulevard de Grenelle(Bd_Grenelle), Boulevard Barbès(Bd_Barbes).
- Période : Données horodatées sur l'année d'étude nettoyée (séries temporelles ramenées au fuseau "Europe/Paris")
- Nombre de lignes : 148,376 lignes ( Dimensions : 148,376 lignes × 11 colonnes)
- Téléchargement : Exécuter le script automatisé de récupération des données depuis l'Open Data (voir section "Lancer le projet")


## Résultats
- Le Boulevard Grenelle conserve un trafic majoritairement fluide avec une capacité limitée plafonnant sous les 400 véhicules/heure et un taux d'occupation restant quasi systématiquement inférieur à 40 %.
- Le Boulevard Barbès subit les congestions les plus sévères, enregistrant des taux d'occupation prolongés atteignant 45 % à plus de 70 %, bien que son débit maximal plafonne à 550 véhicules/heure.
![alt text](image-4.png)
![alt text](image-5.png)


## Limites
- L'étude repose sur des boucles magnétiques spécifiques ; les résultats traduisent le trafic au niveau du point de comptage et non sur l'intégralité du linéaire de la voie.
- L'absence de données sur la météo, les chantiers temporaires, la régulation des feux tricolores ou les événements exceptionnels limite l'explication sur la cause de certains pics isolés.


## Lancer le projet
```bash
# Création et activation de l'environnement virtuel
uv venv
.venv\Scripts\activate # Sur Windows (.venv/bin/activate sur Mac/Linux)
# Installation des dépendances et téléchargement des données
uv pip install -r requirements.txt
python download_data.py
```
Puis exécuter les notebooks dans l'ordre: 
notebooks/exploration.ipynb
notebooks/nettoyage.ipynb
notebooks/transformation.ipynb
notebooks/visualisation.ipynb

## Utilisation de l'IA
Usages:
- Assistance au débogage du code Python
- Structuration et harmonisation esthétique des graphiques Seaborn/Matplotlib
- Résolution des erreurs d'environnement virtuel, 
- Validation métier des règles de filtrage physique du trafic routier (rejet de l'IQR)
  Modifications apportées : 
- Ajustement systématique des suggestions aux spécificités du projet 
- Vérification manuelle des seuils d'aberration
- Rédaction des synthèses analytiques adaptées au contexte parisien

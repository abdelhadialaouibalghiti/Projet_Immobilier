# 🏡 Analyse territoriale pour l'investissement immobilier

## 1. Titre et Contexte

Ce projet de Data Science croise les **prix de l'immobilier en France** avec plusieurs indicateurs locaux de **qualité de vie** : accès aux soins, sécurité, activités sportives, établissements scolaires, gares et ensoleillement.

L'objectif est d'aider un couple de jeunes retraités souhaitant quitter l'Île-de-France à identifier des communes attractives en province, compatibles avec un budget immobilier, un besoin d'accessibilité familiale et un meilleur cadre de vie.

Le projet s'appuie sur une approche complète de **data engineering analytique** :

- ingestion de jeux de données publics volumineux ;
- nettoyage et standardisation des formats ;
- construction de scores territoriaux ;
- jointure entre données immobilières et indicateurs communaux ;
- visualisations interactives dans Jupyter Notebook.

---

## 2. Architecture des données

### 🔁 Vue d'ensemble du pipeline

```text
DVF brut
  └── dvf_nettoyage.ipynb
        └── dvf_clean.csv
              └── prix.ipynb
                    └── dvf_filtre.csv

BPE Santé 2024
  └── nettoyage_santer.ipynb
        └── sante_score.csv

DVF filtré + Score santé
  └── fusion_dvf_sante.ipynb
        └── dvf_sante_fusion.csv
              └── notebook-final_groupe_AIFOUTE.ipynb

Sécurité, sport, éducation, gares, ensoleillement
  └── notebooks dédiés et widgets d'exploration territoriale
```

### 📓 Rôle exact des notebooks

| Notebook | Rôle dans le pipeline | Sorties principales |
|---|---|---|
| `dvf_nettoyage.ipynb` | Charge un échantillon du fichier DVF compressé, conserve les colonnes utiles, nettoie les valeurs foncières, surfaces, types de biens et calcule le `prix_m2`. Les biens aberrants sont filtrés selon le prix au m² et la surface. | `dvf_clean.csv` |
| `prix.ipynb` | Met en place des widgets de filtrage immobilier par budget maximal, nombre minimal de pièces et type de bien. Le résultat filtré est sauvegardé pour les étapes suivantes. | `dvf_filtre.csv` |
| `nettoyage_santer.ipynb` | Exploite la Base Permanente des Équipements 2024 pour isoler les équipements de santé, les agréger par commune et construire un `score_sante` normalisé entre 0 et 1, avec une catégorisation `niveau_sante`. | `sante_score.csv` |
| `fusion_dvf_sante.ipynb` | Harmonise les identifiants géographiques, puis fusionne les biens immobiliers filtrés avec les scores santé. | `dvf_sante_fusion.csv` |
| `clean.ipynb` | Nettoie les données de délinquance communales, conserve l'année la plus récente disponible, agrège le nombre de faits par commune et construit un `score_securite` normalisé. | `delinquance_clean.csv`, `delinquance_score.csv` |
| `widgets_activite_sportive.ipynb` | Analyse les équipements sportifs et les établissements scolaires. Le notebook filtre les activités pertinentes pour des retraités, exclut l'Île-de-France et propose des cartes/graphes interactifs. | Widgets, cartes Folium, graphiques Matplotlib |
| `notebook-final_groupe_AIFOUTE.ipynb` | Notebook de restitution finale. Il combine les modules santé/prix, sport/éducation, gares et ensoleillement dans une expérience interactive orientée décision. | Visualisations interactives finales |
| `securiter.ipynb` | Notebook présent dans le dossier mais sans logique exploitable dans son état actuel. La logique sécurité est implémentée dans `clean.ipynb`. | Aucune sortie détectée |

---

## 3. Résultats clés

Le notebook final met en avant plusieurs axes de décision pour comparer les territoires :

- **🏥 Santé & immobilier** : classement dynamique des 25 communes ayant les meilleurs scores de santé selon le budget, le nombre de pièces et le type de logement sélectionné.
- **🏘️ Accessibilité immobilière** : filtrage des transactions DVF pour identifier les biens compatibles avec le profil du persona, notamment sur la valeur foncière, la surface et le prix au m².
- **🎾 Sport & vie locale** : analyse des départements les mieux équipés en activités adaptées à des retraités, comme piscine, tennis, golf, randonnée, yoga ou pétanque.
- **🏫 Accueil familial** : intégration des établissements scolaires afin d'évaluer les zones adaptées à la venue des enfants et petits-enfants.
- **🚆 Mobilité ferroviaire** : recherche d'une commune par nom ou code postal, calcul des distances aux gares avec la formule de Haversine et affichage des gares proches sur carte Folium.
- **☀️ Climat** : visualisation des départements les plus ensoleillés via un classement interactif et une carte choroplèthe Plotly.

Ces résultats permettent d'aller au-delà du seul prix immobilier en construisant une lecture multicritère du territoire : **budget, santé, sécurité, mobilité, loisirs, famille et climat**.

---

## 4. Stack Technique

### 🐍 Python & Data Processing

- `pandas` : ingestion, nettoyage, agrégation et jointures de données.
- `numpy` : transformation et normalisation des scores.
- `re`, `unicodedata`, `unidecode` : normalisation textuelle, suppression des accents et appariement de communes.
- `difflib` : recherche floue de noms de communes.
- `math` : calcul de distance géographique avec Haversine.

### 📊 Visualisation & Analyse

- `matplotlib` : histogrammes, bar charts et distributions.
- `seaborn` : exploration statistique des données de sécurité.
- `plotly.express` : classements interactifs et cartes choroplèthes.
- `folium` : cartes interactives.
- `folium.plugins.MarkerCluster` : regroupement des marqueurs géographiques.

### 🗺️ Géospatial

- `geopandas` : manipulation de données géographiques.
- `shapely.geometry.Point` : création de géométries ponctuelles.
- GeoJSON des départements français pour la cartographie de l'ensoleillement.

### 🧩 Interactivité Notebook

- `ipywidgets` : sliders, menus déroulants, boutons et tableaux de bord interactifs.
- `IPython.display` : rendu dynamique des widgets, cartes et graphiques.

### 🌐 Données & APIs

- Données DVF pour les transactions immobilières.
- Base Permanente des Équipements 2024 pour les indicateurs de santé.
- Données de délinquance communales pour le score de sécurité.
- Données d'équipements sportifs et annuaire de l'éducation.
- Données de gares voyageurs.
- Données d'ensoleillement par département.
- API `geo.api.gouv.fr` pour la recherche de communes et départements.

---

## 5. Auteur

**Abdelhadi Alaoui Balghiti - MSc Data Engineering & Cloud**

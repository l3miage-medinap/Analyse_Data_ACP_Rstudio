# TP Analyse et fouille de données

**Auteurs :** Pierre Médina & Soroush Soleymani

Projet d'analyse et de fouille de données réalisé en R sur un jeu de données de 200 communes décrites par des variables socio-économiques et géographiques (densité, revenu médian, taux de chômage, part de diplômés du supérieur, prix au m², part des plus de 65 ans, temps de trajet, équipements, région, type de commune, transport principal).

L'objectif est de caractériser les profils de communes, d'identifier les structures principales du jeu de données et les associations entre variables qualitatives, puis d'en tirer des recommandations.

## 🧰 Technologies et librairies

- **Langage :** R
- **Librairies :**
  - `FactoMineR` (ACP, AFC, HCPC)
  - `factoextra` (visualisations)
  - `dplyr` (manipulation des données)
  - `corrplot` (matrice de corrélations)

## 📁 Structure du projet

\`\`\`text
├── communes.csv                          # Jeu de données (200 communes, 12 variables)
├── Pierre_MEDINA_Soroush_SOLEYMANI.Rmd   # Source R Markdown (code + analyse)
├── Pierre_MEDINA_Soroush_SOLEYMANI.html  # Rapport compilé
├── Pierre_MEDINA_Soroush_SOLEYMANI.pdf   # Rapport compilé
└── README.md
\`\`\`

### Format des fichiers d'analyse

Les trois fichiers \`Pierre_MEDINA_Soroush_SOLEYMANI\` (.Rmd, .html, .pdf) \*\*contiennent exactement la même chose\*\* : ce sont trois formats d'un seul et même rapport.

\- Le fichier \*\*\`.Rmd\`\*\* est la \*\*source\*\* : il contient le code R et le texte de l'analyse (questions 1 à 20 + bonus). C'est le seul fichier à modifier ou à exécuter pour reproduire l'analyse.
\- Les fichiers \*\*\`.html\`\*\* et \*\*\`.pdf\`\*\* sont des \*\*compilations\*\* de cette source, générés automatiquement lors du \`knit\` du Rmd. Ils permettent de consulter le rapport (code, résultats et graphiques) directement dans le navigateur ou dans n'importe quel lecteur PDF, sans avoir à exécuter le code.

Concrètement : si vous voulez explorer ou relancer l'analyse, ouvrez le \`.Rmd\` ; si vous voulez simplement la lire, ouvrez le \`.html\` ou le \`.pdf\`.

## 🔬 Méthodologie

Le projet couvre la chaîne complète d'une analyse exploratoire de données :

1. **Import et vérification des types** : lecture du CSV, conversion des variables qualitatives (`region`, `type_commune`, `transport_principal`) en facteurs.
2. **Nettoyage** : suppression des lignes contenant des valeurs manquantes (8 lignes sur 200, soit moins de 5 %) ; conservation de 192 communes.
3. **Statistiques descriptives** : moyennes, min/max et écarts-types de chaque variable numérique.
4. **Transformation de la densité** : la distribution étant très asymétrique, un passage au logarithme (`LogDensite`) la rend à peu près symétrique, indispensable pour une ACP fondée sur les corrélations linéaires.
5. **Matrice des corrélations** : identification des liaisons les plus fortes (prix au m² ↔ part de diplômés : 0,86 ; prix au m² ↔ équipements : 0,86 ; densité ↔ équipements : 0,84).
6. **Tableau de contingence et profils-lignes** : croisement du type de commune et du transport principal.
7. **ACP normée** (FactoMineR) avec variables illustratives (qualitatives et densité brute en supplémentaires).
8. **Test du chi²** sur le tableau de contingence (conditions de validité vérifiées : tous les effectifs théoriques ≥ 5).
9. **AFC** (Analyse Factorielle des Correspondances) sur le tableau type de commune × transport.
10. **HCPC** : classification hiérarchique sur les résultats de l'ACP, avec 5 classes retenues automatiquement.

## 📊 Principaux résultats

### ACP : deux axes retenus (~77 % de la variance)

- **Axe 1, « attractivité urbaine »** (51,8 % de variance) : structuré par les équipements, le prix au m², la part de diplômés et la densité.
- **Axe 2, « fragilité socio-démographique »** (25 % de variance) : lié au taux de chômage, au vieillissement et au temps de trajet (enclavement).

L'axe 1 sépare nettement les communes **rurales** (côté négatif) des communes **urbaines** (côté positif), les communes **périurbaines** occupant une position intermédiaire. La région ne structure pas le plan : c'est le type de commune qui compte.

### Mobilité selon le territoire

- La **voiture** domine en zones rurales (76,5 %) et périurbaines (63,4 %), contre 18,9 % en zones urbaines.
- Les **transports en commun** sont massivement le fait des communes urbaines (49,1 % du profil urbain).
- Le test du chi² confirme la dépendance entre type de commune et transport (χ² = 64,8 ; p < 0,001), et l'AFC l'illustre : l'axe 1 (90,9 % de l'inertie) oppose « Urbaine / Transports en commun » à « Rurale / Voiture ».

### Classification (HCPC)

5 classes émergent : un pôle rural profond enclavé, un pôle rural moins enclavé, deux pôles périurbains de transition (l'un vulnérable, l'autre favorisé) et un pôle urbain dense et riche orienté vers les transports en commun.



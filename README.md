# Projet médias géo-localisés
## Contexte et Objectif
Rennes Métropole souhaite améliorer l’expérience des touristes à Rennes en leur offrant un transport en commun confortable et qui n’est pas surchargé. Le résultat attendu de ce travail consiste à découvrir les zones de Rennes à forte densité de touristes. Autrement dit, extraire des points d'intérêts candidats intéressants dans la ville de Rennes pour anticiper toute congestion eventuelle.

## Outil utilisé : Knime
Knime est un logiciel de ​data mining disponible gratuitement. Le lien suivant https://www.knime.org/downloads/overview permet le téléchargement de ce logiciel. Cela demande d’abord une inscription, ensuite tous les installes des différents systèmes d’exploitation sont fournis. Le site officiel de Knime offre une ​documentation assez complète et des ​cours en ligne​ très utiles.

## Données utilisées
La source de données dans cette étude sera un jeu de données contenant une description des photos de Rennes postées sur Flickr. Il s’agit d’une large collection de photographies géolocalisées. Chaque photo est décrite comme un tuple : ​(id photo, id photographer, latitude, longitude, tags, title/description, dates)​.

## Nettoyage de données
Etant donnée que les données fournies sont brutes, une première étape de nettoyage est indispensable, afin d’éviter de travailler sur des données trompeuses. Pour faire cela, il faut appliquer un noeud ​«GroupBy» ​sur la totalité des colonnes du jeu de données. Par conséquent, un grand nombre de lignes dupliquées ont été éliminées. Au départ, on avait 54800 lignes, après l’élimination de doublons le jeu de données ne comporte maintenant que 4195 lignes.

## Résultats
### Vue globale du workflow
![Imgur](https://i.imgur.com/pf1ZoD8.png)
### Statistiques
On peut déjà observer quelques remarques sur ces photos collectées et sur le comportement des touristes aussi. La plupart des photos sont :
- assez récentes (prises en 2017)
- prises pendant le 1er trimestre de l’année, plus précisément en mois de mars
- prises au milieu du mois, alors que les autres jours du mois connaissent une activité de prise de photo plus ou moins stable et moins forte
- Prises le lundi ou le dimanche, alors que les autres jours de la semaine connaissent une activité de prise de photo plus ou moins stable et moins forte
### Clustering

#### K-means avec 20 clusters
Le workflow commence par la lecture des données et l’élimination des doublons comme évoqué précédemment. Ensuite, dans sa partie supérieure un ​clustering​ k-means​ est appliqué et affiché sur une carte :
![Imgur](https://i.imgur.com/6AwnoZS.png)
On remarque une grande activité de touristes dans le centre de la ville et dans différentes périodes de l’année (les couleurs des points). Il y a également une activité importante dans des points secondaires, comme sur l’aéroport ou encore à Cesson-Sévigné.

#### Clustering hiérarchique avec 8 mots clés
L'intérêt de la partie inférieure du workflow est d’organiser les photos selon leurs motifs respectifs. Pour cela, un processus de​traitement des données textuelles​ et un ​clustering hiérarchique​ est appliqué.
![Imgur](https://i.imgur.com/y1KEi5L.png)
Dans un premier temps, on remplace les valeurs manquantes par des chaînes de caractères vides, afin de pouvoir ensuite concaténer les colonnes ​tags et ​title​ (les seules colonnes de données textuelles) dans une seule colonne ​tagsTitle​ . Puis on convertie cette colonne String en Document, et on applique un ensemble de manipulations dédiées aux données textuelles : élimination de la ponctuation, élimination des majuscules, ​racinisation​, élimination des ​stop words (mots vides), etc. Ensuite, on extrait les mots clés les plus importants à partir des documents analysés, et on attribue un score pour chaque ligne du jeu données (chaque photo) et chacun des termes détectés. Au final, en se basant sur ces termes les documents sont transformés en vecteurs de document (​document vectors​).

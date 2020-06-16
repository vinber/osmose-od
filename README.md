# Méthodologie d'intégration d'un fichier opendata dans OSMOSE

## Osmose

Osmose (osmose.openstreetmap.fr) permet :

- de faire tourner sur la base osm des analyses qualités  (que nous appelerons *analysis*)
- de faire des comparaisons pour regroupements entre les données OSM et des données ouvertes (que nous appellerons les *merge*)

**[Plus d'infos sur le wiki](http://wiki.openstreetmap.org/wiki/FR:Osmose)**

## Un fichier OpenData (OD)

Pour la suite, nous devons avoir un fichier **opendata** (LO ou OdbL). Pour la facilité, nous considérerons dans un premier temps que le fichier :

- est correctement encodé,
- ne présente pas de particularités exotiques
- à une adresse url fixe
- ouvrir une page wiki (encore à creuser l'organisation, mais on peut trouver un certain nombre de page déjà faite ici : https://wiki.openstreetmap.org/wiki/France/data.gouv.fr)

Il faudra aussi avoir un numéro pour ce fichier :

- concernant la numérotation des items
- centaine et dizaine définissent le jeu de données
- le millier la thématique
- l'unité la sous partie !


## Osmose-OD (OD pour OpenData)

Osmose-OD va permettre de faire le rapprochement entre les données OSM et les données ouvertes.

De ce rapprochement 4 situations vont apparaître l'interface Osmose-OD :

- des données sont présentes dans le fichier OD, pas dans OSM ;
- des données sont présentes dans OSM, pas dans le fichier OD ;
- les données OSM et du fichier OD sont rapprochées (selon des critères comme la distance, une référence commune ou autre tag) et peuvent être fusionnées,
- le fichier OD a été mis à jour et peut-être rapproché par une clé commune (référence) et peut donc être mis à jour dans OSM,

## Code

Chaque fichier OD nécessite quelques lignes de code permettant de définir :

- les caractérisitiques du fichier OD (encodage, URL, source, millésime, ...)
- les correspondances entre les champs du fichier OD et les tags OSM,
Cette correspondance est soit directement faite dans le fichier merge soit dans un fichier de mapping (qui se concentre sur le rapprochement entre les en-têtes de colonnes et les tags osm).

- à savoir Osmose peut faire beaucoup beaucoup de choses
  - partir de fichiers csv, json, shp, gtfs, y compris dans un zip
		- filtrer le fichier OD sur certains champs,
		- séparer, fusionner, regrouper des champs du fichier OD pour préparer la correspondance avec les tags OSM,
		- ...
- le mode de rapprochement :
	- la conflation en définissant la distance en mètre entre 2 points,
	- une référence commune aux deux POI,
	- un tag commun aux deux POI,
	
- pour chaque fichier rapproché, on va pouvoir montrer :
	- les données manquantes dans OSM et présentes dans la base OD (missing_official)
	- les données manquantes dans la base OD et présentes dans OSM (?????)
	- la "potentielle fusion" des données OSM et de la base OD (possible_merge)
	- une mise à jour faite du coté de la base OD (update_official) -> nécessite une référence commune pour s'assurer du rapprochement

## Listes des merge actuellement en cours - Github
- [ici, dans merge data sur Github](https://github.com/osm-fr/osmose-backend/tree/master/merge_data)

- [et ici dans analysers sur Github](https://github.com/osm-fr/osmose-backend/tree/master/analysers)

## Exemple de codes

- une url pour un code simple mais complet bnls ?

- le nom du jeu de données

		- description du ficher que l'on va récupérer  (attribution et millésime)
			- file (c'est en dur dans osmose
			- fileurl c'est en ligne chez la collectivité
			- sait lire du csv, json, shp, gtfs, y compris dans un zip
				- description également encodage ...
		- dans le load, on définit principalement les lattitude longitude
			- on peut filtrer le fichier sur le champs
		- mapping des colonnes au tag
			- static : cela ne changera pas : exemple amenity=post_box quand tu travailles sur
				- self.source / calculé à partir de l'attribution et millésime défini au dessous
			- mapping : cela peut bouger exemple : le nom, la référence
				- deux types de mapping
					- valeurs d'une colonne directement transposable en valeurs OSM
					- valeurs OSM calculées à partir d'une colonne (par exemple valeurs booléennes transformées en yes / none ou de plusieurs)

		toujours dans mapping, on va s'attaquer au rapprochement
		tags entre accolade ET, entre crochets OU

		mode de rapprochement :
		- conflationDistance : en metre
		- soit la référence
		- soit un tag (exemple d'un tag wikipédia)



# Osmose OD, nouveau frontend ?

Pour éviter de faire un doublon de ce qui existe sur osmose actuellement, il est nécessaire de :

- définir comment on catégorise les fichiers OD.

Proposition de reprendre la catégorisation faite par data.gouv :

- Agriculture et alimentation
- Culture
- Economie et emploi
- Éducation et recherche
- International et europe
- Logement, développement durable et énergie
- Santé et social
- Société
- Territoires, transports et tourisme


# Redéfinir les catégories, les items et class d'osmose OD

- quelles catégories / 9 catégories
- quelle classification

peut-être se baser sur une lecture des principales bases de données géocodées et ouvertes

# la numérotation dans osmose-QA

- les milliers sont la catégorie
- centaine et dizaine définissent le jeu de données
- l'unité définit le résultat du rapprochement
    - présent dans osm pas dans od
	- présent dans od pas dans osm
	- fusion -> conflation ou autres modes de rapprochements
	- mise à jour se fait sur la référence rentrée sans OSM, la base OD est mise à jour, on le voit sur osmose si tag de niveau 1

- créer des catégories pour OD
	- par thématiques, selon le wiki ?
	- jusqu'à 9, (à tester pour plus avant de l'imaginer)
- dans catégories, des items sur les centaine, dizaines et unités
- chaque item à des class
	- sous division des items en class (raisons différentes)
	- on peut filter soit par items, soit par class

Cette classe est un numéro qui doit être unique, pas encore compris comment on le définit


la classe peut-être le territoire, ce qui est aujourd'hui en partie le cas.


On peut filtrer les analyses et merge par :

- tag associés au merge ou analyses (plusieurs tags possibles, 1 seul en filtre)
- filtre en ligne ou josm (automatique en fonction du retour osmose)

Utiliser docker ALCD

Build docker image

    in docker directory :
    docker build -t alcd .

Preparation des données L1C: 

	Copier l'image L1C d'intérêt sur : 
		path_configuration["global_chains_paths"][L1C]/TILE_NAME/S2A_MSIL1C…

Preparation des fichiers de configuration:

	Copier les fichiers de conf de ALCD/docker/conf/* vers ALCD/parameters_files

Run docker image:

	docker run --rm -it -v /path/to/data:/data -v /path/to/ALCD:/ALCD alcd:latest /bin/bash

Depuis le répertoire ALCD, lancer les étapes: 

	0. Initialisation: 
		python all_run_alcd.py -f True -s 0 -l TILE_NAME -d DATE -u True
	1. Ouvrir et editer les shapefiles :
		Ouvrir dans QGIS /path/to/data/BDD_v0/TILE_NAME_TILE_ID_DATE/In_data/Masks
		S'adaider des images dans /path/to/data/BDD_v0/TILE_NAME_TILE_ID_DATE/Intermediate pour remplir les masques de points :
			- NDWI
			- RGB
			- ...
	2. Premiere itération: 
		python all_run_alcd.py -f True -s 1
	3. Voir Résultats dans : 
		/path/to/data/BDD_CNES/BDD_v0/TILE_NAME_TILE_ID_DATE/Out
	4. Retour à l'etape 1 pour affiner

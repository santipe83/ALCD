Utiliser docker ALCD

Build docker image

    in docker directory :
    docker build -t alcd .

Preparation des données L1C: 
	Copier l'image L1C d'intérêt sur : 
		path_configuration["global_chains_paths"][L1C]/TILE_NAME/S2A_MSIL1C…

Preparation des fichiers de configuration:
	Copier les fichiers de conf de ALCD/docker/conf/* vers ALCD/parameters_files

Run docker image
	docker run --rm -it -v /path/to/data:/data -v /path/to/ALCD:/ALCD alcd:latest /bin/bash

Depuis le répertoire ALCD, lancer les étapes: 
	0. Initialisation: 
		python all_run_alcd.py -f True -s 0 -l TILE_NAME -d 20200124 -u True
	1. Ouvrir et editer les shapefiles
	2. Premiere itération: 
		python all_run_alcd.py -f True -s 1
	3. Voir Résultats dans : 
		/path/to/data/BDD_CNES/BDD_v0/T19KFT_19KFT_20200124/Out
	4. Retour à l'etape 1 pour affiner

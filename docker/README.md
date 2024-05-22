Utiliser docker ALCD

Build docker image. In docker directory : `docker build -t alcd .`

Prepare L1C data, copy input L1C into `path_configuration["global_chains_paths"]["L1C"]/TILE_NAME/S2A_MSIL1C…`

Prepare configuration, copy configuration files from `ALCD/docker/conf/*` into `ALCD/parameters_files`

Run docker image: `docker run --rm -it -v /path/to/data:/data -v /path/to/ALCD:/ALCD alcd:latest /bin/bash`

In the new shell, got to `/ALCD`: 

	1. init: `python all_run_alcd.py -f True -s 0 -l TILE_NAME -d DATE -u True`
	2. Open and edit shapefiles in QGIS: `/path/to/data/BDD_v0/TILE_NAME_TILE_ID_DATE/In_data/Masks`. The images available in `/path/to/data/BDD_v0/TILE_NAME_TILE_ID_DATE/Intermediate` are useful:
		- NDWI
		- RGB
		- ...
	3. First iteration: `python all_run_alcd.py -f True -s 1`
	4. See results in:  `/path/to/data/BDD_CNES/BDD_v0/TILE_NAME_TILE_ID_DATE/Out`
	5. Go back to 2. in order to converge on a satisfying image segmentation.

# ALCD Docker  
 
## Build docker image.  
In the directory containing the dockerfile:
```shell
docker build -t alcd .
```

## Prepare L1C data.
Copy input L1C into `path_configuration["global_chains_paths"]["L1C"]/TILE_NAME/S2A_MSIL1C…`

## Prepare configuration.

Edit `conf/paths_configuration.json` in order to add the location of the TILE you want to process:
```json
  "tile_location": {
    "T31TCH": "31TCH",
	"TILE_NAME": "tile_id"
  }
```
Copy configuration files from `ALCD/docker/conf/*` into `ALCD/parameters_files`

## Run docker image: 
```shell
docker run --rm -it -v /path/to/data:/data -v /path/to/ALCD:/ALCD alcd:latest /bin/bash
```

In the new shell, got to `/ALCD`:  

### 0. Initialize
```shell
python all_run_alcd.py -f True -s 0 -l TILE_NAME -d DATE -u True
```

### 1. Open and edit shapefiles in QGIS
The shapefile coresponding to the  `/path/to/data/BDD_v0/TILE_NAME_TILE_ID_DATE/In_data/Masks`.  
The images available in `/path/to/data/BDD_v0/TILE_NAME_TILE_ID_DATE/Intermediate` are useful:
- NDWI
- RGB
- ...  
![](screen/step2.PNG?raw=true "Step 2, File tree")  
Toggle edition on the shapefile you want to add points to  
![](screen/step2-edit.PNG?raw=true "Step 2, Toggle edit of shapefile")  
Add a point by selecting the right tool and clicking on the image.  
![](screen/step2-add.PNG?raw=true "Step 2, Add marker on map")  
![](screen/step2-clic.PNG?raw=true "Step 2, Validate added point")  

### 2. First iteration
```shell
python all_run_alcd.py -f True -s 1
```

### 3. Analyse results
Check the resulting image in `/path/to/data/BDD_CNES/BDD_v0/TILE_NAME_TILE_ID_DATE/Out`.

### 4. Iterate until completion
Go back to Step 1. in order to help ALCD converge into a satosfying image segmentation by adding/removing points to class.

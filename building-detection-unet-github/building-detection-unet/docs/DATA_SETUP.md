# Guide de configuration des donnees

## WHU Building Dataset

### Telechargement

Le dataset WHU Building est disponible sur le site officiel de Wuhan University :
http://gpcv.whu.edu.cn/data/building_dataset.html

Utiliser la version "Aerial Image Building Dataset".

### Structure attendue dans Google Drive

```
Mon Drive/
`-- Data/
    |-- Train/
    |   |-- Image/    <- 5734 images PNG
    |   `-- Mask/     <- 5732 masques PNG (binaires)
    |-- Val/
    |   |-- Image/    <- 1228 images PNG
    |   `-- Mask/     <- 1228 masques PNG
    |-- Test/
    |   |-- Image/
    |   `-- Mask/
    `-- drone_images/
        |-- OAM_Kinshasa_img0.tif
        |-- OAM_Kinshasa_img1.tif
        `-- OAM_Kinshasa_img2.tif
```

## Images Drone Kinshasa (OpenAerialMap)

Les images GeoTIFF de Kinshasa sont disponibles sur https://openaerialmap.org/

### Metadonnees des images utilisees

| Image | Dimensions      | CRS       | Resolution      |
|-------|----------------|-----------|-----------------|
| img0  | 1349 x 877 px  | EPSG:4326 | 0.000004 deg/px |
| img1  | 1827 x 1184 px | EPSG:4326 | 0.000004 deg/px |
| img2  | 1116 x 2228 px | EPSG:4326 | 0.000004 deg/px |

## Note sur le .gitignore

Les donnees brutes (images, masques, modeles) sont exclues du depot Git car trop volumineuses.
Seuls les notebooks et le code sont versionnes.
Pour partager les donnees, utiliser Google Drive ou Hugging Face Datasets.

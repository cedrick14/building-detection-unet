# Detection de Batiments a partir d'Images Drone - U-Net

Segmentation semantique binaire de batiments sur des images drone GeoTIFF a l'aide d'une architecture U-Net entrainee sur le WHU Building Dataset.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-orange)](https://pytorch.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

---

## Description

Ce projet detecte automatiquement les batiments dans des images drone haute resolution (GeoTIFF) en utilisant un reseau de neurones convolutionnel U-Net. Les resultats sont exportes en :
- Masques GeoTIFF georeferencies (prets pour QGIS/ArcGIS)
- Cartes de probabilite (float32 GeoTIFF)
- Polygones vectorises (GeoPackage / Shapefile)

Zone d'application : images drone de la ville de Kinshasa (RDC), issues du projet OpenAerialMap (OAM).

---

## Structure du projet

```
building-detection-unet/
|
|-- notebooks/
|   |-- train_colab.ipynb                # Entrainement U-Net sur WHU Dataset (Google Colab + GPU)
|   `-- predict_colab_geotiff.ipynb      # Inference sur images drone GeoTIFF (Kinshasa)
|
|-- data/
|   |-- Train/
|   |   |-- Image/                       # Images d'entrainement (PNG/TIF)
|   |   `-- Mask/                        # Masques binaires correspondants
|   |-- Val/
|   |   |-- Image/
|   |   `-- Mask/
|   |-- Test/
|   |   |-- Image/
|   |   `-- Mask/
|   `-- drone_images/                    # Images GeoTIFF drone (Kinshasa)
|       |-- OAM_Kinshasa_img0.tif
|       |-- OAM_Kinshasa_img1.tif
|       `-- OAM_Kinshasa_img2.tif
|
|-- output/                              # Genere automatiquement a l'entrainement
|   `-- best_model.pth
|
|-- predictions/                         # Genere automatiquement a l'inference
|   |-- OAM_Kinshasa_img0_mask.tif
|   |-- OAM_Kinshasa_img0_proba.tif
|   |-- OAM_Kinshasa_img0_result.png
|   `-- batiments_kinshasa.gpkg
|
|-- docs/
|   `-- DATA_SETUP.md
|
|-- requirements.txt
`-- README.md
```

---

## Architecture - U-Net

```
Input (3, 512, 512)
    |
    |-- Encoder (Downsampling)
    |   |-- DoubleConv(3   -> 64)   + MaxPool
    |   |-- DoubleConv(64  -> 128)  + MaxPool
    |   |-- DoubleConv(128 -> 256)  + MaxPool
    |   `-- DoubleConv(256 -> 512)  + MaxPool
    |
    |-- Bottleneck: DoubleConv(512 -> 1024)
    |
    `-- Decoder (Upsampling + Skip Connections)
        |-- ConvTranspose + DoubleConv(1024 -> 512)
        |-- ConvTranspose + DoubleConv(512  -> 256)
        |-- ConvTranspose + DoubleConv(256  -> 128)
        `-- ConvTranspose + DoubleConv(128  -> 64)
                |
    Output: Conv(64 -> 1) -> Sigmoid -> Masque binaire (512, 512)
```

Parametres : ~31M | GPU requis : Tesla T4 (16 GB) recommande

---

## Dataset - WHU Building Dataset

| Split | Images | Masques |
|-------|--------|---------|
| Train | 5 734  | 5 732   |
| Val   | 1 228  | 1 228   |

- Format : PNG / TIF
- Taille des tuiles : 512 x 512 px
- Masques : binaires (0 = fond, 255 = batiment)

---

## Hyperparametres d'entrainement

| Parametre       | Valeur         |
|-----------------|----------------|
| Taille image    | 512 x 512      |
| Batch size      | 4              |
| Epoques max     | 50             |
| Learning rate   | 1e-4           |
| Early stopping  | 10 epoques     |
| Optimiseur      | AdamW (wd=1e-4)|
| Scheduler       | CosineAnnealing|
| Loss            | BCE + Dice (50/50)|

---

## Resultats

| Metrique     | Valeur  |
|--------------|---------|
| Dice Score   | 0.8593  |
| IoU Score    | 0.7967  |
| Epoque (best)| 7       |

---

## Utilisation

### 1. Entrainement (train_colab.ipynb)

Ouvrir dans Google Colab avec GPU T4 active.

```
Execution -> Modifier le type d'execution -> T4 GPU
```

Structure requise dans Google Drive :

```
Mon Drive/
`-- Data/
    |-- Train/Image/  (5734 images)
    |-- Train/Mask/   (5732 masques)
    |-- Val/Image/    (1228 images)
    `-- Val/Mask/     (1228 masques)
```

Le modele est sauvegarde dans `Mon Drive/output/best_model.pth`.

### 2. Inference GeoTIFF (predict_colab_geotiff.ipynb)

Structure requise :

```
Mon Drive/
|-- output/best_model.pth
`-- Data/drone_images/
    |-- OAM_Kinshasa_img0.tif
    |-- OAM_Kinshasa_img1.tif
    `-- OAM_Kinshasa_img2.tif
```

Sorties generees :
- `*_mask.tif` — Masque binaire GeoTIFF georefernce
- `*_proba.tif` — Carte de probabilite (float32)
- `*_result.png` — Visualisation cartographique
- `batiments_kinshasa.gpkg` — Polygones vecteur (GeoPackage, 611 batiments)

---

## Dependances

```bash
pip install -r requirements.txt
```

Ou dans Colab :

```python
!pip install -q albumentations tqdm
!pip install -q rasterio geopandas shapely fiona pyproj
```

---

## Pipeline d'inference

```
Image GeoTIFF
    |
    v
Lecture + Conservation CRS/Transform (rasterio)
    |
    v
Decoupage en tuiles 512x512 px (overlap 64 px)
    |
    v
Normalisation ImageNet -> U-Net -> Sigmoid
    |
    v
Assemblage par moyenne des tuiles
    |
    v
Post-traitement morphologique (fermeture + suppression petits composants)
    |
    v
Export GeoTIFF georefernce + Vectorisation (GeoPackage)
```

---

## Resultats sur Kinshasa

| Image                  | Polygones vecteur |
|------------------------|-------------------|
| OAM_Kinshasa_img0.tif  | 129               |
| OAM_Kinshasa_img1.tif  | 314               |
| OAM_Kinshasa_img2.tif  | 168               |
| Total                  | 611               |

---

## References

- [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://arxiv.org/abs/1505.04597) — Ronneberger et al., 2015
- [WHU Building Dataset](http://gpcv.whu.edu.cn/data/building_dataset.html) — Wuhan University
- [OpenAerialMap](https://openaerialmap.org/) — Images drone open source

---

## Auteur

CEDRICK BELMICH ONDON NKOUA — Projet de detection de batiments par segmentation semantique U-Net sur images drone (Kinshasa, RDC).

---

## Licence

Ce projet est sous licence [MIT](LICENSE).

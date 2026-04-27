# 🏘️ Détection de Bâtiments à partir d'Images Drone — U-Net

> Segmentation sémantique binaire de bâtiments sur des images drone GeoTIFF à l'aide d'une architecture **U-Net** entraînée sur le **WHU Building Dataset**.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-orange)](https://pytorch.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

---

## 📋 Description

Ce projet détecte automatiquement les bâtiments dans des images drone haute résolution (GeoTIFF) en utilisant un réseau de neurones convolutionnel **U-Net**. Les résultats sont exportés en :
- **Masques GeoTIFF géoréférencés** (prêts pour QGIS/ArcGIS)
- **Cartes de probabilité** (float32 GeoTIFF)
- **Polygones vectorisés** (GeoPackage / Shapefile)

Zone d'application : images drone de la ville de **Kinshasa (RDC)** (OpenAerialMap).

---

## 🗂️ Structure du projet

```
building-detection-unet/
│
├── notebooks/
│   ├── train_colab.ipynb                # Entraînement U-Net sur WHU Dataset
│   └── predict_colab_geotiff.ipynb      # Inférence sur images GeoTIFF drone
│
├── data/
│   ├── Train/Image/  &  Train/Mask/
│   ├── Val/Image/    &  Val/Mask/
│   ├── Test/Image/   &  Test/Mask/
│   └── drone_images/
│       ├── OAM_Kinshasa_img0.tif
│       ├── OAM_Kinshasa_img1.tif
│       └── OAM_Kinshasa_img2.tif
│
├── output/
│   └── best_model.pth                   # Meilleur modèle sauvegardé
│
├── predictions/                         # Résultats de l'inférence
│   ├── *_mask.tif                       # Masque GeoTIFF géoréférencé
│   ├── *_proba.tif                      # Carte de probabilité
│   ├── *_result.png                     # Visualisation cartographique
│   └── batiments_kinshasa.gpkg          # Polygones (GeoPackage)
│
├── requirements.txt
└── README.md
```

---

## 🏗️ Architecture — U-Net (31M paramètres)

```
Input (3, 512, 512)
    ├── Encoder: DoubleConv [64 → 128 → 256 → 512] + MaxPool
    ├── Bottleneck: DoubleConv(512 → 1024)
    └── Decoder: ConvTranspose + Skip Connections [512 → 256 → 128 → 64]
                        ↓
    Conv(64→1) → Sigmoid → Masque binaire (512, 512)
```

---

## 📊 Dataset — WHU Building Dataset

| Split | Images |
|-------|--------|
| Train | 5 734  |
| Val   | 1 228  |

Format PNG/TIF, tuiles **512×512 px**, masques binaires (0/255).

---

## ⚙️ Hyperparamètres d'entraînement

| Paramètre    | Valeur           |
|--------------|------------------|
| Taille image | 512 × 512 px     |
| Batch size   | 4                |
| Époques max  | 50               |
| LR           | 1e-4             |
| Early stop   | 10 époques       |
| Optimiseur   | AdamW (wd=1e-4)  |
| Scheduler    | CosineAnnealing  |
| Loss         | BCE + Dice (50/50)|

---

## 📈 Résultats

| Métrique       | Valeur     |
|----------------|------------|
| **Dice Score** | **0.8593** |
| **IoU Score**  | **0.7967** |

---

## 🚀 Utilisation

### 1. Entraînement — `train_colab.ipynb`
Ouvrir dans Google Colab avec GPU T4. Monter Google Drive et placer les données dans `Mon Drive/Data/`.

### 2. Inférence — `predict_colab_geotiff.ipynb`
Nécessite `best_model.pth` et les images GeoTIFF dans `Mon Drive/Data/drone_images/`.

---

## 📦 Installation

```bash
pip install torch torchvision albumentations tqdm
pip install rasterio geopandas shapely fiona pyproj opencv-python-headless
```

---

## 🗺️ Résultats sur Kinshasa

| Image                  | Polygones |
|------------------------|-----------|
| OAM_Kinshasa_img0.tif  | 129       |
| OAM_Kinshasa_img1.tif  | 314       |
| OAM_Kinshasa_img2.tif  | 168       |
| **Total**              | **611**   |

---

## 📚 Références

- [U-Net (Ronneberger et al., 2015)](https://arxiv.org/abs/1505.04597)
- [WHU Building Dataset](http://gpcv.whu.edu.cn/data/building_dataset.html)
- [OpenAerialMap](https://openaerialmap.org/)

---

## 👤 Auteur

**BEQUIET** — Détection de bâtiments par segmentation U-Net sur images drone, Kinshasa (RDC).

## 📝 Licence

[MIT](LICENSE)

# 📁 Guide de configuration des données

## WHU Building Dataset

### Téléchargement
Le dataset WHU Building est disponible sur le site officiel de Wuhan University :
http://gpcv.whu.edu.cn/data/building_dataset.html

### Structure attendue dans Google Drive

```
Mon Drive/
└── Data/
    ├── Train/
    │   ├── Image/    ← 5734 images PNG
    │   └── Mask/     ← 5732 masques PNG (binaires)
    ├── Val/
    │   ├── Image/    ← 1228 images PNG
    │   └── Mask/     ← 1228 masques PNG
    └── drone_images/
        ├── OAM_Kinshasa_img0.tif
        ├── OAM_Kinshasa_img1.tif
        └── OAM_Kinshasa_img2.tif
```

## Images Drone Kinshasa (OpenAerialMap)

Les images GeoTIFF de Kinshasa sont disponibles sur [OpenAerialMap](https://openaerialmap.org/).

| Image | Dimensions | CRS | Résolution |
|-------|-----------|-----|-----------|
| img0  | 1349 × 877 px | EPSG:4326 | 0.000004°/px |
| img1  | 1827 × 1184 px | EPSG:4326 | 0.000004°/px |
| img2  | 1116 × 2228 px | EPSG:4326 | 0.000004°/px |

> Les données brutes sont exclues du dépôt Git (.gitignore). Seuls les notebooks et le code sont versionnés.

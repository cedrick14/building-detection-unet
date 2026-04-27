#  Données du projet

## WHU Building Dataset

Le dataset d'entraînement n'est **pas inclus** dans ce dépôt en raison de sa taille (plusieurs Go).

### Téléchargement

1. Rendez-vous sur : http://gpcv.whu.edu.cn/data/building_dataset.html
2. Téléchargez le dataset "Aerial Imagery Dataset"
3. Décompressez et organisez selon la structure ci-dessous

### Structure attendue

```
Data/
├── Train/
│   ├── Image/    ← 5734 fichiers PNG (512×512 px)
│   └── Mask/     ← 5734 masques binaires PNG
├── Val/
│   ├── Image/    ← 1228 fichiers
│   └── Mask/     ← 1228 fichiers
└── Test/
    ├── Image/
    └── Mask/
```

### Images drone Kinshasa

Les images GeoTIFF proviennent d'**OpenAerialMap** (données ouvertes) :
- https://openaerialmap.org/

Fichiers utilisés :
- `OAM_Kinshasa_img0.tif` (1349 × 877 px, EPSG:4326)
- `OAM_Kinshasa_img1.tif` (1827 × 1184 px, EPSG:4326)
- `OAM_Kinshasa_img2.tif` (1116 × 2228 px, EPSG:4326)

Placez ces fichiers dans `Data/drone_images/`.

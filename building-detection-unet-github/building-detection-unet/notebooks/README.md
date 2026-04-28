# Notebooks

## train_colab.ipynb

Entrainement du modele U-Net sur le WHU Building Dataset via Google Colab (GPU T4).

Etapes :
1. Verification GPU + installation dependances
2. Montage Google Drive
3. Configuration chemins & hyperparametres
4. Dataset + augmentations (albumentations)
5. Architecture U-Net (31M parametres)
6. Loss BCE+Dice, optimiseur AdamW, scheduler CosineAnnealing
7. Boucle d'entrainement avec early stopping & sauvegarde automatique
8. Courbes d'apprentissage (Loss / Dice / IoU)
9. Apercu des predictions sur la validation

Resultats : Dice = 0.8593 | IoU = 0.7967

---

## predict_colab_geotiff.ipynb

Inference du modele entraine sur des images drone GeoTIFF (Kinshasa, RDC).

Etapes :
1. GPU + installation dependances geospatiales (rasterio, geopandas)
2. Montage Google Drive
3. Configuration chemins + verification metadonnees spatiales
4. Chargement U-Net entraine
5. Fonctions d'inference (decoupage en tuiles 512x512 + overlap 64px)
6. Fonctions geospatiales (lecture GeoTIFF, export raster + vecteur)
7. Prediction sur les 3 images Kinshasa
8. Export GeoPackage global (611 batiments)
9. Visualisation cartographique (4 panneaux)
10. Tableau recapitulatif

# 📓 Notebooks

## train_colab.ipynb
Entraînement du modèle U-Net sur le WHU Building Dataset via Google Colab (GPU T4).

**Étapes :**
1. Vérification GPU + installation dépendances
2. Montage Google Drive
3. Configuration chemins & hyperparamètres
4. Dataset + augmentations (albumentation)
5. Architecture U-Net (31M paramètres)
6. Loss BCE+Dice, optimiseur AdamW, scheduler CosineAnnealing
7. Boucle d'entraînement avec early stopping & sauvegarde auto
8. Courbes d'apprentissage (Loss / Dice / IoU)
9. Aperçu des prédictions sur la validation

**Résultats :** Dice = 0.8593 | IoU = 0.7967

---

## predict_colab_geotiff.ipynb
Inférence du modèle entraîné sur des images drone GeoTIFF (Kinshasa, RDC).

**Étapes :**
1. GPU + installation dépendances géospatiales (rasterio, geopandas)
2. Montage Google Drive
3. Configuration chemins + vérification métadonnées spatiales
4. Chargement U-Net entraîné
5. Fonctions d'inférence (découpage en tuiles 512×512 + overlap 64px)
6. Fonctions géospatiales (lecture GeoTIFF, export raster + vecteur)
7. Prédiction sur les 3 images Kinshasa
8. Export GeoPackage global (611 bâtiments)
9. Visualisation cartographique (4 panneaux)
10. Tableau récapitulatif

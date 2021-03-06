# sen2classification

sen2classification is a Python library for automatic land cover classification of Sentinel 2 satellite images using machine learning. 
# Features
- support for L1C and L2A Sentinel products
- uses machine learning algorithms in combination with tresholding for classifying land covers
- calculates spectral indices used for tresholding 
- Two classification methods are available:
  - ALCC (Automatic Land Cover Classification)
  - Gradient boosting
- the end result is a classification raster which pixel values range from 0 to 5:
  - 0 = no data
  - 1 = water
  - 2 = low vegetation
  - 3 = high vegetation
  - 4 = soil
  - 5 = built up

*Clouds are not classified, for optimal results use images with none or minimal cloud cover*

# ALCC
Automatic Land Cover Classification algorithm uses k-means clustering and tresholding for classifying land cover. The concept of ALCC was first introduced by [Gašparović et al. (2019)](https://www.sciencedirect.com/science/article/pii/S0198971518305891). They used it to classify Landsat-8 images at 30 m spatial resolution. This version of ALCC algorithm is adapted for use on Sentinel 2 images with 10 m spatial resolution.
# Gradient boosting 
Gradient boosting classification is implemented using [xboost](https://xgboost.readthedocs.io/en/latest/) library. Collection of training data is automated by tresholding spectral indices.

# Installation

## Important!
Library dependency that must be installed before installing sen2classification is [rasterio](https://rasterio.readthedocs.io/en/latest/) (and [GDAL](https://gdal.org/) which is rasterio dependency). Installing sen2classification won't install rasterio.

Simplest way to install GDAL and rasterio is by using conda with conda-forge channel
```bash
conda install -c conda-forge gdal rasterio
```

After that use pip to install sen2classification.

```bash
pip install sen2classification
```

## Usage

```python
from sen2classification import ALCC
from sen2classification import GBClassification

# input is Sentinel 2 L2A product folder
alcc = ALCC(product='path to unzipped Sentinel 2 L2A product (.SAFE)', product_type='L2A')
# input is Sentinel 2 L1C product folder
xgb = GBClassification(product='path to unzipped Sentinel 2 L1C product (.SAFE)', product_type='L1C')


# sets up band file paths
alcc.setup_bands()

# swir bands must be resampled to 10m for calculating spectral indices
alcc.resample_swir_to_10m()

# calculate AWEI, NDVI, NDTI and BAEI spectra indices
alcc.calculate_spectral_indices()

# run ALCC algorithm
# final classification raster is saved in current working directory
alcc.run()

# setup and execution is identical for xgb


```



## License
[MIT](https://choosealicense.com/licenses/mit/)
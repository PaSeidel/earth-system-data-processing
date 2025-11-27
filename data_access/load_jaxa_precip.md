# Loading the JAXA Precipication Dataset

## Dataset Overview

### 1.1 Dataset Description

The Global Satellite Mapping of Precipitation (GSMaP) is a global precipitation dataset produced by JAXA's Earth Observation Research Center (EORC). The daily gauge-adjusted product provides precipitation estimates at high spatial resolution covering most of the globe.

**Key Specifications:**
- **Variable:** Daily accumulated precipitation (gauge-adjusted)
- **Spatial Coverage:** Global, 60°N to 60°S
- **Spatial Resolution:** 0.1° × 0.1° (approximately 10 km at equator)
- **Temporal Coverage:** March 2000 - February 2014 (Version 6)
- **Temporal Resolution:** Daily accumulation (00Z to 23Z UTC)
- **Data Format:** Cloud Optimized GeoTIFF (COG) via JAXA Earth API
- **Units:** Precipitation rate (mm/day)

### 1.2 Data Source and Processing

The GSMaP dataset combines data from multiple satellite sources:
- **Primary Sensors:** Multi-band passive microwave and infrared radiometers
- **Satellites:** GPM Core Observatory and constellation satellites
- **Processing Algorithm:** GSMaP algorithm version 6 (product version 3)
- **Gauge Adjustment:** The daily precipitation values are calibrated using NOAA/CPC ground-based gauge measurements to improve accuracy

**Important Note:** Version 6 (product version v03) does not detect snowfall. For datasets covering periods after February 2014, version 7 should be used, which includes improved snowfall detection.

**Citation:**
Kubota, T., et al. (2020): Global Satellite Mapping of Precipitation (GSMaP) products in the GPM era, Satellite precipitation measurement, Springer, https://doi.org/10.1007/978-3-030-24568-9_20

## Gathering Information about the Dataset

On the [JAXA website](https://earth.jaxa.jp/en/data/index.html), one can directly query datasets depending on use, access and relevant information, providing a concise interface to identify the wanted dataset. The sattelite datasets from JAXA can be obtained via __JAXA Earh API__ or __G-Portal__, which are also directly linked on the same page.

### G-Portal
__G-Portal__ seems to be the main access point with the highest level of detail and the biggest selection of datasets. To download datasets from G-Portal, an account needs to be created. In their [documentation](https://gportal.jaxa.jp/gpr/assets/mng_upload/COMMON/upload/GPortalUserManual_en.pdf#page=20.14), it is mentioned that files can be downloaded via SFTP. However, I could not follow their [instructions](https://gportal.jaxa.jp/gpr/assets/mng_upload/COMMON/upload/GPortalUserManual_en.pdf#page=24.12), since it included functionality on the website which was not visible to me. I was thus also unable to follow the [next steps](https://gportal.jaxa.jp/gpr/assets/mng_upload/COMMON/upload/GPortalUserManual_en.pdf#page=27.12), and accessing using a UNIX system (via Colab) did not work. 
There exists, however, a [python interface](https://github.com/sankichi92/gportal-python) for the G-Portal system, which is __unofficial__. Since I did not use this for the main part of this project, will discuss this third-party package later.

Generally, the G-Portal website seems very convoluted, with an outdated documentation-file and does not provide easy access to the required data.

### JAXA Earth API

__JAXA Earth API__ is another access point to some of the datasets JAXA provides. The [website](https://data.earth.jaxa.jp/en/) is structured much more clearly, with a succinct overview over all the datasets and [clear documentation](https://data.earth.jaxa.jp/api/python/) on how to access the data using either Python or JavaScript. Access to the data via this API requires no authentication, just the installation of a python-package, which is also detailed in the [documentation](https://data.earth.jaxa.jp/api/python/v0.1.4/en/quick.html).

The information on the datasets is also readily available. On the main information page for each dataset (here for [Precip (daily)](https://data.earth.jaxa.jp/en/datasets/#/id/JAXA.EORC_GSMaP_standard.Gauge.00Z-23Z.v6_daily)), all relevant information is either mentioned or linked: For example, the [source data](https://eolp.jaxa.jp/GSMaP_Hourly.html) being used to compile the dataset, which contains all relevant technical information.

While it was quite easy to find all relevant information about the dataset and how to use the API, there remains a huge drawback: The API does not provide any endpoints for directly downloading the data. Instead, all data is kept in-memory; there is also a lot of functionality included in the package to directly work with the in-memory data. Despite this, I decided to use the JAXA Earth API to prepare the script, due to the problems with G-Portal described above.

## Dataset formats

The JAXA Earth API provides data in **Cloud Optimized GeoTIFF (COG)** format, which offers several advantages:
- **Cloud-native:** Optimized for streaming and partial reads
- **Standard format:** Compatible with all major GIS software (QGIS, ArcGIS, GDAL)
- **Metadata included:** Georeferencing information embedded in file
- **Efficient:** Supports compression and internal tiling

When loaded via the API, the data structure is:
- **Shape:** (time_steps, height, width, 1) as a numpy array
- **Coordinate system:** WGS84 (EPSG:4326)
- **Data type:** Float32

# Data Access

## Loading required Data from Server

## Saving Data

## Loading Data from File

## G-Portal third-party package

# Scaling Up

There are some limitation coming with the access via __JAXA Earth API__:

## Technical Challenges 

### In-memory data

additional I/O operations make the download slower than it needs to be

### File-formats

currently, the code only supports a single data-format

### Meta-data

Meta data is discarded, code can be adapted to also save meta-data

### Directory structure

For a large number of files, the current directory structure might not be sufficient, can be adapted for specific needs.


## Portal challenges

### Dataset Availability

Via G-Portal, many more datasets are available

### Not enough control

Smallest increment is _daily_ data, however, via G-Portal (as shown in the `extra_jaxa_precip.iynb` notebook), hourly data is available, from which the dataset is derived.
Satellite Image Desertification Estimator
A Google Earth Engine pipeline to estimate desertification using NDVI change, climate, and land use data, adaptable for any region.
Table of Contents

Project Overview
Features
Prerequisites
Setup
Usage
Project Structure
Data Sources
Steps
Contributing
License
Acknowledgements

Project Overview
The Satellite Image Desertification Estimator is a Python-based pipeline that leverages Google Earth Engine (GEE) to assess desertification risk across any region of interest (ROI). It uses satellite-derived data, including NDVI (Normalized Difference Vegetation Index), precipitation, temperature, soil moisture, soil organic carbon, and land use changes, to classify areas as desertified or non-desertified. The pipeline is designed to be adaptable for various ecosystems (e.g., mangroves, forests, grasslands) by incorporating region-specific desertification criteria. This project was developed as of June 13, 2025, and tested on the Sundarbans region in India.
Features

NDVI Change Analysis: Computes NDVI change over time using fused Landsat, Sentinel-2, and MODIS data.
Region-Specific Classification: Applies ecosystem-specific criteria for desertification (e.g., soil moisture for mangroves, deforestation for forests).
Feature Stack: Includes climate (precipitation, temperature), soil (moisture, organic carbon), and land use (deforestation) data.
Random Forest Model: Trains a classifier to predict desertification risk based on synthetic labels.
Generalizability: Designed to work for any ROI with minimal adjustments.

Prerequisites

Python 3.11+: Ensure Python is installed on your system.
Google Earth Engine Account: Sign up for GEE at code.earthengine.google.com and authenticate your account.
GEE Python API: Install the earthengine-api Python package.
Dependencies: Install required Python libraries (listed below).

Setup
1. Clone the Repository
git clone https://github.com/your-username/satellite-image-desertification-estimator.git
cd satellite-image-desertification-estimator

2. Install Dependencies
Create a virtual environment and install the required packages:
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install earthengine-api tqdm numpy

3. Authenticate Google Earth Engine
Run the following command to authenticate your GEE account:
earthengine authenticate

Follow the prompts to log in and copy the authentication token into your terminal.
4. Verify Setup
Ensure GEE is working by running a simple script:
import ee
ee.Initialize()
print(ee.Image(1).getInfo())  # Should print {'type': 'Image', ...}

Usage
1. Configure the Region of Interest (ROI)
Edit the script to define your ROI. For example, for the Sundarbans:
roi = ee.Geometry.Rectangle([88.0, 20.9, 90.0, 22.9])

2. Run the Pipeline
Execute the main script to process the data and train the model:
python main.py

The script will:

Load and fuse satellite data (Steps 1–7).
Compute NDVI change and build a feature stack (Steps 8–9).
Train a Random Forest classifier and evaluate results (Steps 10–12).

3. View Results

Output Logs: Check the console for progress and validation metrics (e.g., valid pixel percentages).
Maps: Export desertification maps to GEE Assets or visualize them in the GEE Code Editor.

Project Structure
satellite-image-desertification-estimator/
├── main.py               # Main script to run the pipeline
├── utils.py              # Helper functions (e.g., gap filling, validation)
├── README.md             # Project documentation
├── requirements.txt      # List of dependencies
└── outputs/              # Directory for exported maps and results

Data Sources
The pipeline uses the following GEE datasets:

NDVI: Landsat 8 (LANDSAT/LC08/C02/T1_L2), Sentinel-2 (COPERNICUS/S2), MODIS (MODIS/061/MOD13Q1).
Precipitation: CHIRPS (UCSB-CHG/CHIRPS/PENTAD).
Temperature: ERA5-Land (ECMWF/ERA5_LAND/MONTHLY).
Land Cover: ESA WorldCover (ESA/WorldCover/v100/2020).
Soil Moisture: SMAP (NASA/SMAP/SPL3SMP_E/005).
Soil Organic Carbon: OpenLandMap (OpenLandMap/SOL/SOL_ORGANIC-CARBON_USDA-6A1C_M/v02).
Deforestation: C3S Land Cover (C3S-LC-L4-LCCS-Map-300m-P1Y).

Steps
The pipeline is divided into 12 steps:

Initialize GEE: Set up GEE and define the ROI.
Load Satellite Data: Fetch Landsat, Sentinel-2, and MODIS data.
Preprocess Data: Apply cloud masking and scaling.
Compute NDVI: Calculate NDVI for each dataset.
Fuse NDVI Data: Combine NDVI from multiple sources with dynamic weights.
Gap Filling: Fill missing pixels using focal mean and global mean.
Reproject and Validate: Ensure consistent resolution (500m) and validate data coverage.
Assign NDVI Images: Validate and assign NDVI for different years (e.g., 2016, 2022).
Feature Stack and Classification: Build a feature stack and classify desertification using ecosystem-specific criteria.
Train Random Forest: Train a classifier using synthetic labels.
Predict and Export: Predict desertification risk and export results.
Evaluate Results: Assess model performance and visualize outputs.

Contributing
Contributions are welcome! Please follow these steps:

Fork the repository.
Create a new branch (git checkout -b feature/your-feature).
Commit your changes (git commit -m "Add your feature").
Push to the branch (git push origin feature/your-feature).
Open a pull request.

License
This project is licensed under the MIT License. See the LICENSE file for details.
Acknowledgements

Google Earth Engine Team: For providing access to satellite data and computational resources.
Copernicus Climate Change Service (C3S): For land cover data.
NASA: For SMAP soil moisture data.
FAO and OpenLandMap: For soil organic carbon data.


# Course outline

## Week 1

This week's lab will provide an introduction to programming concepts in Python:

* Python programs
* data types and structures
* variables
* control flow, loops, and conditional execution
* classes and objects

It will demonstrate these concepts by:

1) creating a Python program that runs as an AI-powered farm assistant using a large language model.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-1_1.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

2) running a short Python program that loads crop yield data collected by harvesters from disk and visualises this data on interactive maps.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-1_2.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

3) practice exercises working with Python data types, data structures and control flow.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-1_practice.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

4) practice exercises that demonstrates how to setup Google Colab to run large language models via Hugging Face.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-1_0_llms.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

## Week 2

These labs introduce data visualisation concepts, selecting different figures to explore a dataset's structure and relationships between variables, using colour to highlight features and patterns, and generating maps from spatial data. You will be working with crop yield data sampled by a header on a harvester from a field in the Western Australia and satellite images from the same field. 

**You do not need a GPU instance in Google Colab for these labs.**

1) introduction to data visualisation and creating interactive figures.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-2_1.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

2) geospatial data visualisation of raster and vector data.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-2_2.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

3) practice exercises generating interactive visualisations using Plotly Express from an Our World in Data article: <a href="https://ourworldindata.org/yields-habitat-loss" target="_blank">*To protect the world’s wildlife we must improve crop yields – especially across Africa*</a>. 

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-2_practice.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

4) practice exercises reading GeoTIFF files storing Sentinel-2 images captured near York, Western Australia in a mix of cloudy and clear sky conditions. Create a new Google Colab notebook and exectute the following code snippet to read the files. 

```{python}
import os
import subprocess

if "data_lab-2_geotiffs_practice" not in os.listdir(os.getcwd()):
    subprocess.run('wget "https://github.com/geog3300-agri3003/lab-data/raw/main/data_lab-2_geotiffs_practice.zip"', shell=True, capture_output=True, text=True)
    subprocess.run('unzip "data_lab-2_geotiffs_practice.zip"', shell=True, capture_output=True, text=True)
    if "data_lab-2_geotiffs_practice" not in os.listdir(os.getcwd()):
        print("Has a directory called data_lab-2_geotiffs_practice been downloaded and placed in your working directory? If not, try re-executing this code chunk")
    else:
        print("Data download OK")
```

## Week 3 

These labs will provide an introduction to file formats for tabular, vector, and raster data; best practice for reading and writing spatial and non-spatial data in Python programs; navigating directories and file systems; and, identifying suitable file formats for different tasks, use cases, contexts (e.g. big data / web environments). You will be working with a range of datasets including satellite images, climate reanalysis products, and crop yield data collected by a harvester from a field in Western Australia.

**You do not need a GPU instance in Google Colab for these labs.**

1) data I/O and file formats.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-3_1.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

2) geospatial data I/O.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-3_2.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

3) practice exercises reading and writing vector data to different formats working with flood maps generated by the European Commission Copernicus Emergency Management Service (EMS) - Rapid Mapping Activations.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-3_practice.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

4) demonstration of partial reads from cloud native geospatial file formats.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-3_cloud_native_geospatial.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>


## Week 4

These labs focus on data wrangling and data transformation. You will implement a range of data transformation and geoprocessing operations including map algebra, spatial and non-spatial data subsetting, zonal statistics, and combining datasets. One lab will focus on raster data and build a data processing workflow to convert satellite images into flood maps following a tropical cyclone event in Fiji. The second lab will turn a satellite image into a vector-tabular dataset of field boundaries with a crop type label and field-average spectral reflectance values. This demonstrates how we can process satellite images into a format ready for machine learning model development. 

**You do not need a GPU instance in Google Colab for these labs.**

1) data wrangling: raster data.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-4_1.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

2) data wrangling: vector and tabular data

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-4_2.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

3) practice exercises performing a range of geometry operations to generate zones for zonal statistics operations to compute summary statistics describing areas flooded and not flooded after a tropical cyclone in event in a satellite image. 

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-4_practice.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

## Week 5

An introduction to machine learning concepts and workflows, how to implement a machine learning workflow in Python, and considerations for machine learning with spatial data. You will implement two machine learning tasks. First, you will use the AgriFieldNet Competition Dataset&nbsp;<a target="_blank" href="https://mlhub.earth/data/ref_agrifieldnet_competition_v1" rel="noopener">(Radiant Earth Foundation and IDinsight, 2022)</a>&nbsp;dataset to develop a crop type classifier for fields in India using Sentinel-2 remote sensing data. Second, you will develop a model to predict maize crop yields in Uganda with Sentinel-2 remote sensing data using the replication data from&nbsp;<a target="_blank" href="https://web.stanford.edu/~mburke/papers/lobell_et_al_AJAE_2019.pdf" rel="noopener">Lobell et al.&nbsp;(2019)</a>.

**You do not need a GPU instance in Google Colab for these labs.**

1) crop type classification

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-5_1.ipynb" target="_blank" rel="noopener"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" /> </a>

2) crop yield prediction

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-5_2.ipynb" target="_blank" rel="noopener"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" /> </a>

## Week 6

An introduction to computer-to-computer communication, client-server architectures, HTTP requests / responses, and web APIs. You will learn how to use Python to make requests to web APIs, use web APIs to download data into their programs, and scrape data from websites. You apply these skills to make a weather data application for a field in Western Australia that sources data from an open weather web API. You will also learn how to use the SpatioTemporal Asset Catalog (STAC) specification and STAC APIs to define and automate searches for geospatial data that meets certain query conditions from various cloud providers.

**You do not need a GPU instance in Google Colab for these labs.**

1) web APIs, client-server architectures, and cloud computing

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-6_1.ipynb" target="_blank" rel="noopener"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" /> </a>

2) SpatioTemporal Asset Collections - retrieving geospatial data from the cloud

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-6_2.ipynb" target="_blank" rel="noopener"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" /> </a>

## Week 7

An introduction to web mapping technologies, highlighting their strengths and limitations for particular contexts, and providing some recipes for requesting web map data using Python.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-7_1.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

## Week 8

An introduction to using UAV images and demonstrating the process to generate science-ready UAV data for answering agricultural questions.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-8_1.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

## Week 9

An introduction to analysing high-resolution UAV data over a cotton trial in New South Wales.

<a href="https://colab.research.google.com/github/geog3300-agri3003/coursebook/blob/main/docs/notebooks/week-9.ipynb" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>
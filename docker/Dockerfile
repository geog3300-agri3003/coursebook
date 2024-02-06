# https://quay.io/repository/jupyter/scipy-notebook?tab=tags
FROM quay.io/jupyter/scipy-notebook:1494233e27cd

# add geospaital and visualisation packages
RUN mamba install --quiet --yes \
    'requests' \
    'bottleneck' \
    'beautifulsoup4' \
    'html5lib' \
    'dask' \
    'numba' \
    'geopandas' \ 
    'dask-geopandas' \
    'pyarrow' \
    'pyogrio' \
    'pysal' \
    'libpysal' \
    'shapely' \
    'folium' \
    'ipyleaflet' \
    'localtileserver' \
    'plotly' \
    'traitlets' \
    'leafmap' \
    'mapclassify' \ 
    'contextily' \
    'cartopy' \
    'panel' \
    'hvplot' \ 
    'holoviews' \ 
    'geoviews' \ 
    'datashader' \ 
    'param' \ 
    'colorcet' \
    'rasterio' \
    'xarray' \
    'rioxarray' \
    'xarray-spatial' \
    'odc-stac' \
    'planetary-computer' \
    'stackstac' \
    'pystac-client' \
    'rasterstats' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# https://github.com/geopandas/geopandas/issues/2442
ENV PROJ_LIB=/opt/conda/share/proj 

# add geospaital and visualisation packages
RUN pip install --quiet --no-cache-dir \
    'gradio-client' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

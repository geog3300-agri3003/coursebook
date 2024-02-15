FROM jmad1v07/geog3300-agri3003-2024

# add geospaital and visualisation packages
RUN pip install --quiet --no-cache-dir \
    'mkdocs==1.5.3' \
    'mkdocs-material==9.5.7' \
    'mkdocs-jupyter==0.24.6' && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
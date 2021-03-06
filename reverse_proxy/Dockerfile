FROM continuumio/miniconda3:4.9.2-alpine
LABEL org.bokeh.demo.maintainer="Bokeh <info@bokeh.org>"

ENV BK_VERSION=2.2.0
ENV PY_VERSION=3.8
ENV NUM_PROCS=4
ENV BOKEH_RESOURCES=cdn

# RUN apt-get install git bash

RUN conda install --yes git

# install packages from conda-forge using mamba

RUN conda config --append channels conda-forge
RUN conda install --yes --quiet mamba
RUN mamba install --yes --quiet python=${PY_VERSION} pyyaml bokeh=${BK_VERSION} "nodejs>=8.8" fsspec hvplot datashader geoviews xarray dask zarr netcdf4 intake intake-xarray metpy requests rasterio==1.1.8
RUN conda clean -ay

# clone the notebooks from repo into examples folker
RUN git clone https://github.com/reproducible-notebooks/Holoviz-Demos.git /bokeh
RUN mkdir -p /examples && cp /bokeh/*.ipynb /examples/ && rm -rf /bokeh

COPY mypanels /examples/


#ADD https://raw.githubusercontent.com/bokeh/demo.bokeh.org/main/index.html /index.html

EXPOSE 5006
EXPOSE 80

CMD panel serve \
    --allow-websocket-origin="*" \
    --num-procs=${NUM_PROCS} \
    /examples/view.ipynb \
    /examples/pie.ipynb \
    /examples/HRRR-Explorer.ipynb \
    /examples/WaveWatch3-Explorer.ipynb

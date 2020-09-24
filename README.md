# osm_completeness
This series of notebooks uses a random forest regression model with google earth engine and python to predict OSM building mapping in afghanistan. notebooks can be adapted for use in any country. built by new light technologies for the world bank.


1. First, an [Observable notebook](https://observablehq.com/@jobelanger/osm-training-set-creation-nepal-example) is used to identify those areas of a given region that already have complete OSM building mapping. These areas are used as a training set for building the regression model, imported in step 2 (Train OSM)
2. `TrainOSM.ipynb` &mdash; (jupyter) create and train a random forest regression model that predicts OSM building footprint area using a variety of satellite imagery sources, including NTL, NDVI, NDBI, GUF, WorldPop, DEM, Landcover, etc. The output model from this notebook will be called in ApplyModel.ipynb.
3. `SplitArea.ipynb` &mdash; (jupyter) create 250-m polygon tessellations over your region of interest from a shapefile. This will allow your area of interest to be assessed for completeness. 
4. `Enrich.ipynb` &mdash; (jupyter) enrich your tessellation AOI (from SplitArea.ipynb) with the data to be used for the model.
5. `ApplyModel.ipynb` &mdash; (jupyter) run the tessellation AOI through the trained model to calculate predicted OSM footprint area
6. Results of `ApplyModel.ipynb` can be visualized in a second [Observable notebook](https://observablehq.com/d/09da0d4f932c9310). Be aware of a 15 MB limit to the file that can be attached to the notebook. For example, Kabul Province was divided into 4 pieces in order to be visualized. 

The conda environment necessary to run the Jupyter notebooks can be installed using the file `envs/wbenv.yml`. To build it enter:<br>
`conda env create --name envname --file wbenv.yml`<br>
in the the anaconda terminal when inside the directory containing the yml file. Windows machines may encounter an issue with the rtree dependency for geopandas, specifically the error `'OSError: could not find or load spatialindex_c-64.dll'` when importing geopandas. The workaround is to find the `core.py` file in the rtree library and change the line:<br>
```python
elif 'conda' in sys.version:
```
to
```python
elif os.path.exists(os.path.join(sys.prefix, 'conda-meta')):

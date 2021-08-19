# How to set up your Python environment

## Doing all the installation stuff

1. Install Miniconda (https://conda.io/miniconda.html).

2. Add the ``conda-forge`` channel as the new default

```
$ conda config --add channels conda-forge
```

3. Use strict channel priority to prevent channel clashes

```
$ conda config --set channel_priority strict
```

4. Create a new ``conda`` environment

```
$ conda create --name umweltdv python=3.7
```

5. Activate the new environment

```
$ conda activate umweltdv
```

6. Install dependencies

```
(umweltdv) $ conda install numpy scipy pandas matplotlib notebook h5py netCDF4 geopandas jupyter_contrib_nbextensions 
```

## Get the course respository

```
$ git clone ...
```

## Start jupyter notebook and get to work

1. Open terminal in directory `umweltdv`
2. Start jupyter
```
$ conda activate umweltdv
(umweltdv)$ jupyter notebook
```


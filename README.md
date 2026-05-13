# GEOL0069 Final Assignment
This repository is the final assignment for the UCL Earth Sciences module GEOL0069 "AI for Earth Observation".

## Steps to install
First, clone the packages from their respective Git repositories:
```
git clone https://github.com/CPOMUCL/GPSat.git
git clone https://github.com/totony4real/DeepRandomFeatures.git
```

From inside `path/to/GPSat`:

```
conda create -n geol0069_gpsat python=3.11
conda activate geol0069_gpsat
python -m pip install -r requirements.txt
python -m pip install -e ./
```

From inside `path/to/DeepRandomFeatures`:
```
conda create -n geol0069_drf python=3.11
conda activate geol0069_drf
python -m pip install -r requirements.txt
python -m pip install -e ./
```


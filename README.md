# GEOL0069 Final Assignment
This repository is the final assignment for the UCL Earth Sciences module GEOL0069 "AI for Earth Observation".

## Project background
### The problem
Satellite altimetry missions use sythetic aperture radar (SAR) to find the surface height measurements of sea ice, open ocean, and many other surface types. Surface height measurements of ocean through cracks in sea-ice (known as leads) can be used to infer the sea level anomaly (SLA) of the ocean under the surface of sea ice. SLA is defined as the sea surface height minus the mean sea surface:

$$ SLA = SSH - MSS $$

This provides an idea of the sea surface's deviation from the mean and can be used in eddy tracking. The process of infering this SLA, however, can be challenging due to the limitations on acquiring SLA datapoints. Not all measurements made by the satellite altimeter at the footprint resolution will contain leads and so an effective interpolation algorithm must be used. The focus of this project is to determine the strengths of different algorithms for this purpose.

Shown in this project is the performance of a linear interpolation algorithm and a machine learning package developed by UCL's Centre for Polar Observation and Modelling (CPOM), known as GPSat.

The process of SAR is shown in the graphics below:

<img src="images/1.png" width="600">
<img src="images/2.png" width="600">
<img src="images/3.png" width="600">
<img src="images/4.png" width="600">

### Environmental assessment
It is important to evaluate the environmental impact of this project. 

#### Satellite launches
The Sentinel 3A, Sentinel 3B, and CryoSat-2 were placed in orbit on N~2~O~4~-powered launch platforms, which have CO_2 emissions of 200 g per kg of fuel. Below is a table to roughly calculate the emissions of each launch.

| Satellite | Vehicle | Estimated fuel mass (kg) | CO~2~ emissions
| -- | -- | -- | -- |
| Sentinel 3A | Rokot | 87130 | 17426 |
| Sentinel 3B | Rokot | 87130 | 17426 |
| Cryosat-2 | Dnepr | 170107 | 34021 |

It is important to consider that while these emissions are very large, they are only a one-time emission. The data used by this project are the same as many other projects, and so it is reasonable to assume the total carbon cost of this project is a marginal one, rather than attributing the entire launch footprint to it.

Similarly, we should determine the CO~2~ emissions associated with interpolation. The interpolations were performed on the UCL CPOM server, with a AMD EPYC 9354P 32-Core Processor and a NVIDIA L4 Tensor Core GPU. The GPU's energy use can be inspected during interpolation, and multiplied by the total interpolation time output by GPSat to give us the energy expenditure of a full month of tracks. Approximate CPU wattage for linear interpolation is determined by multiplying the CPU use of the process by the low thermal design power (TPD) of the CPU, which is in this case 240W. The avg. CPU use during interpolation 8.83%.

| Interpolation | Power (W) | Interpolation time (s) | Power use (kWh) | CO~2~ emissions
| -- | -- | -- | -- | -- |
| Linear | 21.2 | x | x | x |
| GPSat | 28.8 | x | x | x |

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


# GEOL0069 Final Assignment
This repository is the final assignment for the UCL Earth Sciences module GEOL0069 "AI for Earth Observation".

## Project background
### The problem
Satellite altimetry missions use sythetic aperture radar (SAR) to find the surface height measurements of sea ice, open ocean, and many other surface types. Surface height measurements of ocean through cracks in sea-ice (known as leads) can be used to infer the sea level anomaly (SLA) of the ocean under the surface of sea ice. SLA is defined as the sea surface height minus the mean sea surface:

$$ SLA = SSH - MSS $$

This provides an idea of the sea surface's deviation from the mean and can be used in eddy tracking. The process of infering this SLA, however, can be challenging due to the limitations on acquiring SLA datapoints. Not all measurements made by the satellite altimeter at the footprint resolution will contain leads and so an effective interpolation algorithm must be used. The focus of this project is to determine the strengths of different algorithms for this purpose.

Shown in this project is the performance of a linear interpolation algorithm and a machine learning package developed by UCL's Centre for Polar Observation and Modelling (CPOM), known as GPSat. The GPSat package's code can be found [here](https://github.com/CPOMUCL/GPSat). We will use these to interpolate over 300 SAR tracks from the satellite missions CryosSat-2 and Sentinel-3 from January 2020.

The process of SAR is shown in the graphics below:

1. The SAR satellite, either CryoSat-2 [2] or Sentinel 3 [5], emits bursts of radar pulses which echo from the ground. Each pulse is emitted at a different angle, meaning that when it reaches the detector after reflecting, it will have a different Doppler shift to all other pulses.
<img src="images/1.png" width="600">
The effect of this is that one Doppler "strip" receives a series of pulses, all at different Doppler shifts, giving several measurements of altitude, and improving the resolution of the altimeter. CryoSat-2 and Sentinel 3 have along-track footprints of 300 m.
<img src="images/2.png" width="600">
SSH data can be acquired over sea ice leads, which are cracks in sea ice. However, the over sea ice floes (water covered by the ice), these data cannot be obtained.
<img src="images/3.png" width="600">
With what data we do have from SAR, we can use linear interpolation or Gaussian processes to estimate the SSH, or to second order, SLA, at unobservable points.
<img src="images/4.png" width="600">

### Environmental assessment
It is important to evaluate the environmental impact of this project. Below is an analysis of the CO<sub>2</sub> emissions of the launch of the satellites used and of the energy used in this project's computation.

#### Satellite launches
The Sentinel 3A, Sentinel 3B, and CryoSat-2 were placed in orbit on N<sub>2</sub>O<sub>4</sub>-powered launch platforms, which have CO_2 emissions of 200 g per kg of fuel [7]. Below is a table to roughly calculate the emissions of each launch.

| Satellite | Vehicle | Estimated fuel mass (kg) | CO<sub>2</sub> emissions
| -- | -- | -- | -- |
| Sentinel 3A | Rokot | 87130 [4] | 17426 |
| Sentinel 3B | Rokot | 87130 | 17426 |
| Cryosat-2 | Dnepr | 170107 [3] | 34021 |

While these emissions are very large, they are only a one-time emission. The data used by this project are the same as many other projects, and so it is reasonable to assume the total carbon cost of this project is a marginal one, rather than attributing the entire launch footprint to it.

#### Emissions due to computation
Similarly, we should determine the CO<sub>2</sub> emissions associated with interpolation. The interpolations were performed on the UCL CPOM server, with a AMD EPYC 9354P 32-Core Processor and a NVIDIA L4 Tensor Core GPU. The GPU's energy use can be inspected during interpolation, and multiplied by the total interpolation time output by GPSat to give us the energy expenditure of a full month of tracks. Approximate CPU wattage for linear interpolation is determined by multiplying the CPU use of the process by the low thermal design power (TPD) of the CPU, which is in this case 240W. The avg. CPU use during interpolation 8.83%.

Note that the recorded times are only of interpolation times, and not of any intermediary steps in memory or storage. They do, however, represent the dominant factors in energy usage and so this is a reasonable approximation to make.

This gives us an energy use in kWh, which we can then use to estimate the CO<sub>2</sub> emitted. The daily average carbon intensity of the UK at time of writing is 116 gCO<sub>2</sub> / kWh [1]. While London's carbon intensity is higher than the national average, UCL's documented sustainability initiatives [6] mean that the energy mix might be closer to the national average.

| Interpolation | Power (W) | Interpolation time (s) | Energy use (kWh) | CO<sub>2</sub> emissions (g) |
| -- | -- | -- | -- | -- |
| Linear | 21.2 | 105.10 | $6.19 \times 10^{-4} | 0.0718 |
| GPSat | 28.8 | 11060 | 8.85 \times 10^{-2} | 10.3 |

The total CO<sub>2</sub> emissions from the computation of this project is 10.3 g. This comes from analysing 300 tracks from Jan 2020, but the month's data contains 5000 tracks. The costs can easily scale when analysing larger tranches of data. If scaled up to the month, the emissions become 171 g.

## Getting started
Local use of these notebooks require **a machine with a GPU**, and spatiotemporal scalar field data in the format shown in `preprocessing.ipynb`. Once you have these prerequisites
### Steps to install
This repo requires the installation of the GPSat library. This is done by cloning the repo:
```
git clone https://github.com/CPOMUCL/GPSat.git
```
And then following the installation instructions. This requires the creation of a virtual env, preferably with conda.

From inside `path/to/GPSat`:
```
conda create -n env python=3.11
conda activate env
python -m pip install -r requirements.txt
python -m pip install -e ./
```

Now you have an environment set up to run the notebooks in this repository. To run the notebooks, navigate to a separate directory and clone this repo:
```
git clone https://github.com/max-henderson404/geol0069_final_assignment.git
```
Then you are good to go. For further instructions on use, please consult the tutorial video below.

## Tutorial video

This video walks you through the steps required to download, install and successfully run the notebooks in this project.

[
    ![Watch the tutorial](https://img.youtube.com/vi/l_hcUUSME98/0.jpg)
](https://youtu.be/l_hcUUSME98)

## References
1. carbonintensity.org.uk. Carbon Intensity API. Carbon Intensity https://carbonintensity.org.uk.
2. Bouzinac, C. CryoSat-2 Product Handbook. 58 https://earth.esa.int/eogateway/documents/20142/37627/CryoSat-Baseline-D-Product-Handbook.pdf (2019).
3. Wade, M. Dnepr. Encyclopedia Astronautica http://www.astronautix.com/d/dnepr.html (2019).
4. Wade, M. Rokot. Encyclopedia Astronautica https://web.archive.org/web/20130404163150/http://www.astronautix.com/lvs/rokot.htm (2013).
5. Quartly, G. & Aublanc, J. Sentinel-3 SRAL/MWR       Land User Handbook. (2025).
6. UCL. UCL Annual Sustainability Report 2024-2025. https://www.ucl.ac.uk/sustainable/sites/sustainable/files/ucl_annual_sustainability_report_2024-2025.pdf (2025).
7. Brown, T. F. M., Bannister, M. T., Revell, L. E., Sukhodolov, T. & Rozanov, E. Worldwide Rocket Launch Emissions 2019: An Inventory for Use in Global Models. Earth and Space Science 11, e2024EA003668 (2024).

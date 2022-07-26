# Description of the Dockerfile
Base image:
- ubuntu:focal

Additional softwares:
- GRASS GIS 7.8.6-1~focal1
- Python 3 (lastest)
- R (lastest)
- python3-numpy
- python3-scipy
- python3-pandas
- python3-rpy2
- python3-pip
- jupyter
- jupyterlab


# Related usages: 
- Project "Mosquimap"



# GRASS GIS Add-ons
A section of the Dockerfile is dedicated for the installation of GRASS GIS Add-ons for OBIA processing. If you don't need them, remove of comment this section. If you need other, please adapt accordingly. 
### DOCKER FILE DESCRIPTION 
## Base image: Tensorflow - Cuda driver 11.2.0 with Ubuntu Focal (20.04); 
## Softwares: Python3 (numpy, scipy, pandas, OpenCV, Gdal), Jupyter Lab

FROM tensorflow/tensorflow:2.9.0-gpu-jupyter

LABEL maintainer="tais.grippa@ulb.be"

ARG DEBIAN_FRONTEND=noninteractive

# Add user
RUN useradd -ms /bin/bash tais

# Update & upgrade system
RUN apt-get -y update && \
    apt-get -y upgrade
RUN apt-get install -y --no-install-recommends apt-utils

# Setup locales
RUN apt-get install -y locales
RUN echo LANG="en_US.UTF-8" > /etc/default/locale
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# Install Numpy, Scikit-learn, Pandas, Natsort 
RUN apt-get install -y --no-install-recommends \
        python3-numpy \
        python3-sklearn \
        python3-pandas \
        python3-natsort

# Install Gdal, Graphviz and Pydot (for tf.plot_model), OpenCV
RUN apt-get -y update 
RUN apt-get install -y --no-install-recommends \
	python3-gdal \
	python3-graphviz \
	python3-pydot \
	python3-opencv

# Install Jupyterlab
RUN pip install jupyterlab 

# Install and allow Jupyter extension to be able to connect via Google Colab
RUN pip install --upgrade jupyter_http_over_ws>=0.0.7
RUN jupyter serverextension enable --py jupyter_http_over_ws
#RUN pip install google.colab 
RUN pip install tensorflow_hub
  
ENV JUPYTER_ENABLE_LAB=yes
ENV PATH="$HOME/.local/bin:$PATH"

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents
# kernel crashes.
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

# Reduce image size
RUN apt-get autoremove -y && \
    apt-get clean -y
	
USER tais
WORKDIR /home/tais

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root", \
    "--NotebookApp.allow_origin='https://colab.research.google.com'", \
    "--NotebookApp.port_retries=0"]

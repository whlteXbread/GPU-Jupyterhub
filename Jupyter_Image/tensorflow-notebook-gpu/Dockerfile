# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

FROM base-notebook-gpu

MAINTAINER Samir Jabari <samir.jabari@fau.de>
#MAINTAINER aburnap@mit.edu
#MAINTAINER Jupyter Project <jupyter@googlegroups.com>

USER root

# Install all OS dependencies for fully functional notebook server
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    curl \
    freeglut3-dev \
    libcupti-dev \
    libcurl3-dev \
    libfreetype6-dev \
    libzmq3-dev \
    pkg-config \
    unzip \
    rsync \
    inkscape \
    jed \
    libsm6 \
    libxext-dev \
    libxrender1 \
    lmodern \
    pandoc \
    vim \
    libpng-dev \
    g++ \
    gfortran \
    libgl1-mesa-glx \
    libffi-dev \
    libhdf5-dev \
    libjpeg-dev \
    liblcms2-dev \
    libopenblas-dev \
    liblapack-dev \
    libssl-dev \
    libtiff5-dev \
    libwebp-dev \
    nano \
    zlib1g-dev \
    qt5-default \
    libvtk6-dev \
    libopenexr-dev \
    libdc1394-22-dev \
    libavcodec-dev \
    libavformat-dev \
    libswscale-dev \
    libtheora-dev \
    libvorbis-dev \
    libxvidcore-dev \
    libx264-dev \
    yasm \
    libopencore-amrnb-dev \
    libopencore-amrwb-dev \
    libv4l-dev \
    libxine2-dev \
    libtbb-dev \
    libeigen3-dev \
    ant \
    default-jdk \
    bc \
    cmake \
    doxygen \
    ffmpeg \
    libvips \
    libvips-dev \
    libvips-tools \ 
    texlive-latex-extra \
    dvipng  && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN conda install --yes\
    Pillow \
    h5py \
    ipykernel \
    matplotlib \
    seaborn \
    numpy \
    pandas \
    scipy \
    sympy \
    nose \
    setuptools \
    tk \
    bcolz \
    Cython \
    path.py \
    sphinx \
    wheel \
    pygments \
    Flask \
    scikit-image \
    scikit-learn \
    tqdm \
    statsmodels \
    pydicom \
    tensorboardX \
    tensorflow-tensorboard \
    plotly \
    ipython \
    tensorflow-gpu && \
    fix-permissions $CONDA_DIR

# Installing pytorch
RUN conda install pytorch torchvision cudatoolkit=10.0 -c pytorch && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR

# Installing OpenCV
RUN conda install opencv -c conda-forge && \
    fix-permissions $CONDA_DIR

RUN python3.6 -m pip install --upgrade --no-cache-dir \
    zmq \
    imgaug \
    kaggle \
    pretrainedmodels \
    torchsummary \
    skorch && \
    fix-permissions /home/$NB_USER

RUN python3.6 -m pip install --upgrade --no-cache-dir --ignore-installed  --pre \
    jupyter-tensorboard &&\
    fix-permissions /home/$NB_USER


ENV CUDA_HOME /usr/local/cuda-10.0
ENV LD_LIBRARY_PATH /usr/local/cuda-10.0:/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH
ENV LIBRARY_PATH $LD_LIBRARY_PATH

# Install facets which does not have a pip or conda package at the moment
RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    rm -rf facets && \
    fix-permissions $CONDA_DIR

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions /home/$NB_USER

EXPOSE 5000 6006

USER $NB_USER


# cleanup
 #RUN 	apt-get autoremove -y && \
  # apt-get autoclean && \
  # apt-get clean && \
 #rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

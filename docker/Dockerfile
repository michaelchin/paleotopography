#
# docker build -f Dockerfile -t paleotopography .
# docker run -p 8888:8888 -it --rm -v`pwd`:/workspace paleotopography
#
#DO NOT USE CONDA!!! PYGPLATES MAY CONFLICT WITH SOME OF THE LIBRARIES IN CONDA.

FROM ubuntu:20.04

#Install ubuntu libraires and packages
RUN apt-get update -y
RUN apt-get install -y wget python3-pip libgeos-dev libproj-dev proj-data proj-bin   

RUN pip3 install --upgrade pip && \
    pip3 install --no-cache-dir scipy matplotlib numpy jupyter pyproj 
RUN pip3 install --no-cache-dir cartopy notebook h5py pyshp healpy netCDF4 scikit-image xarray joblib

#install pygplates
RUN apt-get update -y
RUN wget https://www.earthbyte.org/webdav/ftp/earthbyte/GPlates/pygplates/pygplates-ubuntu-focal_2.2_1_amd64.deb -O /pygplates-ubuntu-focal_2.2_1_amd64.deb
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata
RUN apt-get install -y qt5-default libqt5opengl5-dev libqt5svg5-dev libqt5xmlpatterns5-dev libqwt-qt5-dev
RUN apt-get install -y /pygplates-ubuntu-focal_2.2_1_amd64.deb
RUN rm /pygplates-ubuntu-focal_2.2_1_amd64.deb

RUN apt-get install -y gmt gmt-gshhg

RUN apt-get install -y git-all
RUN pip3 install --no-cache-dir --upgrade git+https://github.com/EarthByte/PlateTectonicTools

ENV PYTHONPATH ${PYTHONPATH}:/usr/lib:/usr/lib/pygplates/revision28

EXPOSE 8888

# Add Tini
ENV TINI_VERSION v0.9.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]
CMD ["/startup.sh"]

WORKDIR /workspace

ADD ./startup.sh /startup.sh
RUN chmod +x /startup.sh


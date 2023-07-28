ARG DEBIAN_FRONTEND=noninteractive

FROM ubuntu:20.04 as builder

ARG DEBIAN_FRONTEND
ARG OTB_VERSION=7.2.0

RUN apt-get update -y
RUN DEBIAN_FRONTEND=noninteractive apt-get install --quiet --yes --no-install-recommends \
        # basic system installs
        build-essential \
        python3 \
        python3-pip \
        wget \
        ca-certificates \
        unzip \
        # packages needed for compilation
        cmake \
        swig \
        ninja-build \
        python3-dev \
        git\
        python3-numpy \
        libgdal-dev \
        freeglut3-dev \
        libboost-date-time-dev \
        libboost-filesystem-dev \
        libboost-graph-dev \
        libboost-program-options-dev \
        libboost-system-dev \
        libboost-thread-dev \
        libinsighttoolkit4-dev \
        libopenthreads-dev \
        libossim-dev \
        libtinyxml-dev \
        libmuparser-dev \
        libmuparserx-dev \
        libfftw3-dev \
        libgsl-dev \
        libgtk-3-dev \
        libopencv-dev \
        g++ make


# Build OTB
RUN mkdir -p /root/otb-build/build && \
    mkdir -p /install/otb && \
    cd /root/otb-build && \
    wget -q https://www.orfeo-toolbox.org/packages/archives/OTB/OTB-7.2.0.zip -O /tmp/OTB.zip && \
    unzip /tmp/OTB.zip && \
    cd /root/otb-build/build && \
    cmake \
        "-DBUILD_COOKBOOK:BOOL=OFF" \
        "-DBUILD_EXAMPLES:BOOL=OFF" \
        "-DBUILD_TESTING:BOOL=OFF" \
        "-DBUILD_SHARED_LIBS:BOOL=ON" \
        "-DOTB_WRAP_PYTHON:BOOL=ON" \
        "-DOTB_USE_MUPARSER:BOOL=ON" \
        "-DOTB_USE_MUPARSERX:BOOL=ON" \
        "-DOTB_USE_SIFTFAST:BOOL=ON" \
        "-DOTB_USE_SPTW:BOOL=ON" \
        "-DOTB_USE_SSE_FLAGS:BOOL=ON" \
        "-DOTB_USE_LIBKML:BOOL=ON" \
        "-DOTB_USE_OPENCV:BOOL=ON" \
        "-DModule_OTBTemporalGapFilling:BOOL=ON" \
        "-DModule_SertitObject:BOOL=ON" \
        "-DModule_otbGRM:BOOL=ON" \
        "-DCMAKE_BUILD_TYPE=Release" \
        "-DCMAKE_BUILD_PARALLEL_LEVEL=4" \
        -DCMAKE_INSTALL_PREFIX="/install/otb" -GNinja .. && \
    ninja install && \
    rm -rf /root/otb-build /tmp/OTB.zip

FROM ubuntu:20.04

ARG DEBIAN_FRONTEND

WORKDIR /app

RUN apt-get update -y
RUN apt-get install --quiet --yes --no-install-recommends \
        python3 \
        python-is-python3 \
        python3-pip \
        python3-lxml \
        gdal-bin \
        file \
        libpython3.8 \
        libinsighttoolkit4.13 \
        libopenthreads21 \
        libossim1 \
        libtinyxml2.6.2v5 \
        libmuparser2v5 \
        libmuparserx4.0.7 \
        libfftw3-3 \
        libgsl23 \
        libgslcblas0 \
        libgtk-3-0 \
        libopencv-dev \
        libpcre2-32-0

COPY requirements.txt /app
RUN python3 -m pip install "numpy==1.20"
RUN python3 -m pip install "gdal==3.0.4"
RUN python3 -m pip install -r requirements.txt


COPY --from=builder /install/otb /usr/local
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
ENV OTB_APPLICATION_PATH=/usr/local/lib/otb/applications/
ENV PYTHONPATH=$PYTHONPATH:/usr/local/lib/otb/python:/usr/local/lib/python3.8/site-packages/
ENV PATH=/usr/local/app:/usr/local/bin:$PATH

ENTRYPOINT [ "" ]

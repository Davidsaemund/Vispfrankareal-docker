FROM vispci/vispci:ubuntu-20.04
LABEL Name=vispfrankareal Version=0.0.1
RUN apt-get update 

RUN apt-get install -y  \
    build-essential \
    cmake \
    git \
    libpoco-dev \
    libeigen3-dev \
    libssl-dev \
    libusb-1.0-0-dev \
    pkg-config \
    libgtk-3-dev \
    cmake-curses-gui \
    libglfw3-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    libudev-dev \
    sudo

#Get Franka library
RUN cd ${HOME}/visp-ws \
    && mkdir 3rdparty \
    && cd ${HOME}/visp-ws/3rdparty \
    && git clone --recursive https://github.com/frankaemika/libfranka \
    && cd libfranka \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=OFF ..\
    && cmake --build . \
    && cpack -G DEB \
    && sudo dpkg -i libfranka*.deb \
    && make \
    && sudo make install
    
#Get librealsense
RUN cd ${HOME}/visp-ws/visp/3rdparty \
    && git clone https://github.com/IntelRealSense/librealsense.git \
    && cd librealsense \
    && sudo ./scripts/setup_udev_rules.sh \
    && mkdir build \
    && cd build \
    && cmake .. -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=Release \
    && sudo make uninstall \
    && make clean \ 
    && make \
    && sudo make install

# Rebuild Visp

RUN cd ${HOME}/visp-ws/visp \
    && git pull \
    && cd ${HOME}/visp-ws/visp-build \
    && cmake ../visp \
    && make -j4 



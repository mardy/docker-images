FROM ubuntu:16.04
LABEL Description="Ubuntu Xenial with newer gcc from ppa:ubuntu-toolchain-r/test, Qt 5.12 and git"
ARG QT_VERSION=5.15.2
ARG QT_MODULES=
ARG QT_HOST=linux
ARG QT_TARGET=desktop
ARG QT_ARCH=


# Dependencies of Qt
RUN apt-get -y update && apt-get install -y \
    curl \
    libdbus-1-3 \
    libexpat1 \
    libfontconfig1 \
    libfreetype6 \
    libgl1-mesa-glx \
    libglib2.0-0 \
    libx11-6 \
    libx11-xcb1 \
    libxcb-icccm4 \
    libxcb-image0 \
    libxcb-keysyms1 \
    libxcb-render-util0 \
    libxcb-xinerama0 \
    software-properties-common # for add-apt-repository

# Need a newer python for aqt
RUN add-apt-repository -y 'ppa:savoury1/python' && apt-get -y update
RUN apt-get install -y python3.9 python3.9-distutils
RUN update-alternatives --install /usr/bin/python3 python /usr/bin/python3.5 1
RUN update-alternatives --install /usr/bin/python3 python /usr/bin/python3.9 2 && update-alternatives --set python /usr/bin/python3.9
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && python3 get-pip.py --force-reinstall

RUN pip3 install aqtinstall

RUN aqt install-qt --outputdir /usr/local/Qt ${QT_HOST} ${QT_TARGET} ${QT_VERSION} ${QT_ARCH} -m ${QT_MODULES}
ENV QTDIR /usr/local/Qt/${QT_VERSION}/gcc_64
ENV PATH="/usr/local/Qt/${QT_VERSION}/gcc_64/bin/:/usr/local/Qt/Tools/QtCreator/bin:${PATH}"
ENV QML2_IMPORT_PATH /usr/local/Qt/${QT_VERSION}/gcc_64/qml/

# Restore original python version as default
RUN update-alternatives --set python /usr/bin/python3.5

# Cmake
RUN curl -L -O 'https://github.com/Kitware/CMake/releases/download/v3.23.2/cmake-3.23.2-linux-x86_64.sh' && \
    chmod a+x cmake-3.23.2-linux-x86_64.sh && \
    ./cmake-3.23.2-linux-x86_64.sh --prefix=/usr --skip-license

# Core libraries
RUN apt-get install -y \
    libglu1-mesa-dev libxrender1 libxkbcommon-x11-0

# Compilers, plus some most common programming tools
RUN add-apt-repository -y 'ppa:ubuntu-toolchain-r/test'
RUN add-apt-repository -y 'ppa:mardy/qbs-on-lts'
RUN apt-get -y update && apt-get install -y \
    g++ g++-5 g++-6 g++-7 g++-8 g++-9 \
    gcc gcc-5 gcc-6 gcc-7 gcc-8 gcc-9 \
    build-essential \
    git \
    pkg-config \
    qbs \
    xvfb

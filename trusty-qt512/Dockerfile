FROM ubuntu:14.04
ARG qtversion_major=5.12
ARG qtversion_full=5.12.6
LABEL Description="Ubuntu trusty with newer gcc from ppa:ubuntu-toolchain-r/test, Qt 5.12 and git"

# Dependencies of Qt
RUN apt-get -y update && apt-get install -y \
    curl \
    libasound2-dev \
    libfontconfig1-dev \
    libfreetype6-dev \
    libglu1-mesa-dev \
    libgtk-3-dev \
    libx11-dev \
    libxext-dev \
    libxfixes-dev \
    libxi-dev \
    libxrender-dev \
    libxcb1-dev \
    libx11-xcb-dev \
    libxcb-glx0-dev \
    libxkbcommon-x11-dev \
    python2.7 \
    xz-utils \
    software-properties-common # for add-apt-repository

# Compilers, plus some most common programming tools
RUN add-apt-repository -y 'ppa:ubuntu-toolchain-r/test'
RUN add-apt-repository -y 'ppa:mardy/qbs-on-lts'
RUN apt-get -y update && apt-get install -y \
    g++ g++-5 g++-6 g++-7 g++-8 \
    gcc gcc-5 gcc-6 gcc-7 gcc-8 \
    build-essential \
    git \
    pkg-config \
    qbs \
    xvfb

RUN curl -L -O "https://download.qt.io/official_releases/qt/${qtversion_major}/${qtversion_full}/single/qt-everywhere-src-${qtversion_full}.tar.xz" && \
    tar xJf qt-everywhere-src-${qtversion_full}.tar.xz && \
    rm -f qt-everywhere-src-${qtversion_full}.tar.xz
COPY 0001-QQuickTreeModelAdaptor1-fix-updating-of-ModelIndex-r.patch 0001-QQuickTreeModelAdaptor1-fix-updating-of-ModelIndex-r.patch
RUN cd qt*/qtquickcontrols && patch -p1 < ../../0001-QQuickTreeModelAdaptor1-fix-updating-of-ModelIndex-r.patch && cd ../.. && rm *.patch
RUN cd qt* && ./configure -opensource -confirm-license -nomake examples -nomake tests && make -j8 module-qtbase && make install && cd .. && rm -rf qt*
ENV QTDIR /usr/local/Qt-${qtversion_full}/gcc_64
ENV PATH="/usr/local/Qt-${qtversion_full}/gcc_64/bin/:${PATH}"

# Cmake
RUN curl -L -O 'https://github.com/Kitware/CMake/releases/download/v3.13.2/cmake-3.13.2-Linux-x86_64.sh' && \
    chmod a+x cmake-3.13.2-Linux-x86_64.sh && \
    ./cmake-3.13.2-Linux-x86_64.sh --prefix=/usr --skip-license


FROM ubuntu:14.04
LABEL Description="Ubuntu trusty with newer gcc from ppa:ubuntu-toolchain-r/test, Qt 5.12 and git"

# Dependencies of the Qt offline installer
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
    software-properties-common # for add-apt-repository

COPY qtifwsilent.qs qtifwsilent.qs
RUN curl -L -O 'https://download.qt.io/official_releases/qt/5.12/5.12.0/qt-opensource-linux-x64-5.12.0.run' && \
    chmod +x qt-opensource-linux-x64-5.12.0.run && \
    QT_INSTALL_DIR=/usr/local/Qt ./qt-opensource-linux-x64-5.12.0.run --platform minimal --script qtifwsilent.qs && \
    rm -f qt-opensource-linux-x64-5.12.0.run qtifwsilent.qs
ENV QTDIR /usr/local/Qt/5.12.0/gcc_64
ENV PATH="/usr/local/Qt/5.12.0/gcc_64/bin/:${PATH}"

# Cmake
RUN curl -L -O 'https://github.com/Kitware/CMake/releases/download/v3.13.2/cmake-3.13.2-Linux-x86_64.sh' && \
    chmod a+x cmake-3.13.2-Linux-x86_64.sh && \
    ./cmake-3.13.2-Linux-x86_64.sh --prefix=/usr --skip-license

# Compilers, plus some most common programming tools
RUN add-apt-repository -y 'ppa:ubuntu-toolchain-r/test'
RUN apt-get -y update && apt-get install -y \
    g++ g++-5 g++-6 g++-7 g++-8 \
    gcc gcc-5 gcc-6 gcc-7 gcc-8 \
    build-essential \
    git \
    pkg-config \
    xvfb

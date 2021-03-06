FROM python:3.6.6-slim-stretch

LABEL maintainer="Jose Truyol <jose@namku.cl>"

ARG RUNTIME_DEPS='ca-certificates libpng-dev libjpeg-dev libwebp-dev \
                  libtiff5-dev libopenexr-dev libopenblas-dev \
                 libx11-dev liblapack-dev liblapacke-dev libglib2.0-0'
ARG BUILD_DEPS='apt-utils wget unzip cmake build-essential pkg-config'
ARG LIB_PREFIX='/usr/local'
ARG VERSION='3.4.1'

ENV OPENCV_VERSION=${VERSION}  \
    LIB_PREFIX=${LIB_PREFIX} 

RUN echo "OpenCV: ${OPENCV_VERSION}" \
    && apt-get update && apt-get install -y ${BUILD_DEPS} ${RUNTIME_DEPS} --no-install-recommends \
    && wget https://github.com/Itseez/opencv/archive/${OPENCV_VERSION}.zip -O opencv.zip \
    && wget https://github.com/Itseez/opencv_contrib/archive/${OPENCV_VERSION}.zip -O opencv_contrib.zip \
    && pip install --upgrade pip\
    && pip install numpy \
    && mkdir /opencv \
    && mv opencv.zip opencv_contrib.zip /opencv \
    && cd /opencv \
    && unzip -qq opencv.zip \
    && mv opencv-${OPENCV_VERSION} opencv \
    && unzip -qq opencv_contrib.zip \
    && mv opencv_contrib-${OPENCV_VERSION} opencv_contrib \
    && mkdir opencv/build \
    && cd opencv/build \
    && cp /usr/include/lapacke*.h /usr/include/openblas/ \
    && opencv_cmake_flags="-D CMAKE_BUILD_TYPE=RELEASE \
    -D BUILD_DOCS=OFF \
    -D BUILD_TESTS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_JAVA=OFF \
    -D BUILD_opencv_apps=OFF \
    -D BUILD_opencv_aruco=OFF \
    -D BUILD_opencv_bgsegm=OFF \
    -D BUILD_opencv_bioinspired=OFF \
    -D BUILD_opencv_ccalib=OFF \
    -D BUILD_opencv_datasets=OFF \
    -D BUILD_opencv_dnn_objdetect=OFF \
    -D BUILD_opencv_dpm=OFF \
    -D BUILD_opencv_fuzzy=OFF \
    -D BUILD_opencv_hfs=OFF \
    -D BUILD_opencv_java_bindings_generator=OFF \
    -D BUILD_opencv_js=OFF \
    -D BUILD_opencv_img_hash=OFF \
    -D BUILD_opencv_line_descriptor=OFF \
    -D BUILD_opencv_optflow=OFF \
    -D BUILD_opencv_phase_unwrapping=OFF \
    -D BUILD_opencv_python3=ON \
    -D BUILD_opencv_python_bindings_generator=ON \
    -D BUILD_opencv_reg=OFF \
    -D BUILD_opencv_rgbd=OFF \
    -D BUILD_opencv_saliency=OFF \
    -D BUILD_opencv_shape=OFF \
    -D BUILD_opencv_stereo=OFF \
    -D BUILD_opencv_stitching=OFF \
    -D BUILD_opencv_structured_light=OFF \
    -D BUILD_opencv_superres=OFF \
    -D BUILD_opencv_surface_matching=OFF \
    -D BUILD_opencv_ts=OFF \
    -D BUILD_opencv_xobjdetect=OFF \
    -D BUILD_opencv_xphoto=OFF \
    -D CMAKE_INSTALL_PREFIX=$LIB_PREFIX \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules" \
    && cmake $opencv_cmake_flags .. \
    && make -j $(getconf _NPROCESSORS_ONLN) \
    && cd /opencv/opencv/build \
    && make install \
    && cd / \
    && rm -rf /opencv \
    && apt-get purge -y --auto-remove $BUILD_DEPS \
    && apt-get autoremove -y --purge \
    && apt-get install -y $RUNTIME_DEPS --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* /usr/share/man /usr/local/share/man /tmp/*


CMD ["python3"]
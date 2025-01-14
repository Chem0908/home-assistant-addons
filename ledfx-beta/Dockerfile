ARG BUILD_FROM

FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy Python requirements file
COPY requirements.txt /tmp/

# Extra index for pre build armv7 python wheels
ENV PIP_EXTRA_INDEX_URL=https://www.piwheels.org/simple

# Setup base
ARG BUILD_ARCH
RUN \
    apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libavformat58\
    libasound2-plugins \
    cython3 \
    gcc \
    git \
    libatlas3-base\
    zlib1g-dev \
    portaudio19-dev \
    python3-dev \
    python3 \
    python3-pip \
    nginx \
    unzip\
    \
    # Pillow dependencies for dev branch
    libfreetype6-dev \
    libfribidi-dev\
    libharfbuzz-dev\
    libjpeg-turbo-progs \
    libjpeg62-turbo-dev \
    liblcms2-dev \
    libopenjp2-7-dev \
    tcl8.6-dev \
    tk8.6-dev \
    libtiff5-dev \
    \
    # aubio dependencies
    python3-aubio \
    aubio-tools \
    python3-numpy \
    libavcodec58 \
    libchromaprint1 \
    libavresample4 \
    libavutil56 \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libswresample-dev \
    libswresample3 \
    libavresample-dev \
    libsndfile1-dev \
    librubberband-dev \
    libsamplerate0-dev \
    && rm -fr \
    /var/{cache,log}/* \
    /var/lib/apt/lists/* \
    \
    && pip3 install \
    --verbose --no-cache-dir -r /tmp/requirements.txt \
    && pip3 install \
    --verbose --no-cache-dir git+https://github.com/LedFx/LedFx \
    \
    && rm -rf /usr/local/lib/python3.9/dist-packages/ledfx_frontend \
    && curl -L --max-redirs 10 -o ledfx_frontend_hass.zip https://github.com/YeonV/LedFx-Frontend-v2/releases/latest/download/ledfx_frontend_hass.zip \
    && unzip ledfx_frontend_hass.zip && cp -r ledfx_frontend_hass /usr/local/lib/python3.9/dist-packages/ledfx_frontend \
    \
    && find /usr/local/lib/python3.9/ -type d -name tests -depth -exec rm -rf {} \; \
    && find /usr/local/lib/python3.9/ -type d -name test -depth -exec rm -rf {} \; \
    && find /usr/local/lib/python3.9/ -name __pycache__ -depth -exec rm -rf {} \; \
    && find /usr/local/lib/python3.9/ -name "*.pyc" -depth -exec rm -f {} \; \
    \
    # Remove not needed packages
    && apt-get purge -y \
    build-essential \
    cython3 \
    gcc \
    git \
    libatlas3-base \
    zlib1g-dev \
    python3-dev \
    \
    # Pillow dependencies for dev branch 
    libfreetype6-dev \
    libfribidi-dev \
    libharfbuzz-dev \
    libjpeg-turbo-progs \
    libjpeg62-turbo-dev \
    liblcms2-dev \
    libopenjp2-7-dev \
    tcl8.6-dev \
    tk8.6-dev \
    libtiff5-dev \
    \
    # aubio dependencies 
    libavcodec58 \
    libchromaprint1 \
    libavresample4 \
    libavutil56 \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libswresample-dev \
    libswresample3 \
    libavresample-dev \
    libsndfile1-dev \
    librubberband-dev \
    libsamplerate0-dev \
    unzip \
    \
    && apt-get update && apt-get install -y --no-install-recommends \
    python3-aubio=0.4.9-4+b4 \
    aubio-tools=0.4.9-4+b4 \
    python3-numpy=1:1.19.5-1 \
    libasound2-plugins=1.2.2-2 \
    && apt-get clean -y \
    && apt-get autoremove -y \
    && rm -fr \
    /var/{cache,log}/* \
    /var/lib/apt/lists/* \
    /root/.cache \
    /etc/nginx \
    && find /tmp/ -mindepth 1  -delete \
    && mkdir -p /var/log/nginx \
    && touch /var/log/nginx/error.log 
# Copy root filesystem
COPY rootfs /

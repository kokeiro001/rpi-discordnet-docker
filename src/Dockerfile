FROM kokeiro001/rpi-dotnet:2.0-runtime
MAINTAINER kokeiro001


# install libopus
RUN set -x \
    && apt-get update && apt-get install -y libopus0 \
    && rm -rf /var/lib/apt/lists/* \
    && ln -sf /usr/lib/arm-linux-gnueabihf/libopus.so.0.5.0 /usr/lib/arm-linux-gnueabihf/libopus.so

#install ffmpeg:need key
RUN keyName='deb-multimedia-keyring_2016.8.1_all.deb' \
    && set -x \
    && curl -LO http://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/$keyName \
    && dpkg -i $keyName \
    && rm $keyName \
    && echo "deb http://www.deb-multimedia.org jessie main" > /etc/apt/sources.list.d/deb-multimedia.list \
    && apt-get update && apt-get install -y ffmpeg \
    && rm -rf /var/lib/apt/lists/*


# install libsodium:from source
ENV LIBSODIUM_DOWNLOAD_URL https://github.com/jedisct1/libsodium/releases/download/1.0.3/libsodium-1.0.3.tar.gz

RUN buildDeps='curl gcc libc6-dev make xz-utils' \
    && set -x \
    && apt-get update && apt-get install -y $buildDeps --no-install-recommends \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /usr/src/libsodium \
    && curl -sSL "$LIBSODIUM_DOWNLOAD_URL" -o libsodium.tar.gz \
    && tar -xzf libsodium.tar.gz -C /usr/src/libsodium --strip-components=1 \
    && rm libsodium.tar.gz \
    && cd /usr/src/libsodium \
    && ./configure \
    && make -j`nproc` \
    && make install \
    && cd / \
    && rm -rf /usr/src/libsodium \
    && ln -sf /usr/local/lib/libsodium.so /usr/lib/arm-linux-gnueabihf/libsodium.so \
    && apt-get purge -y --auto-remove $buildDeps

RUN mkdir /app
VOLUME ["/app"]
WORKDIR "/app"
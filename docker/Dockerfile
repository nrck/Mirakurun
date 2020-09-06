ARG ARCH=
FROM ${ARCH}node:14.7.0-buster AS build
WORKDIR /app
ENV DOCKER=YES
ADD . .
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends build-essential && \
    npm install && \
    npm run build && \
    npm install -g --unsafe-perm --production

FROM ${ARCH}node:14.7.0-buster-slim

WORKDIR /app
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        make \
        gcc \
        g++ \
        pkg-config \
        pcscd \
        libpcsclite-dev \
        libccid \
        libdvbv5-dev \
        pcsc-tools \
        dvb-tools \
        build-essential \
        automake \
        pkg-config \
        wget \
        unzip \
        cmake \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    wget --no-check-certificate https://github.com/stz2012/libarib25/archive/master.zip -O ./a.zip && \
    unzip -o a.zip && \
    rm ./a.zip && \
    cd libarib25-master && \
    cmake . && \
    make && \
    make install && \
    cd / && \
    rm -rf libarib25-master && \
    wget --no-check-certificate https://github.com/stz2012/recpt1/archive/master.zip -O ./b.zip && \
    unzip -o b.zip && \
    rm ./b.zip && \
    cd recpt1-master/recpt1 && \
    ./autogen.sh && \
    ./configure --enable-b25 && \
    make && \
    make install && \
    cd / && \
    rm -rf recpt1-master
COPY --from=build /usr/local/lib/node_modules/mirakurun /app
CMD ["./docker/container-init.sh"]
EXPOSE 40772 9229

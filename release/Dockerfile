# Build and install the latest TileDB stable release

# To build:
#   docker build -t tiledb:release
#
# Use the build arg 'enable' to configure optional TileDB components, e.g.:
#   docker build --build-arg enable=s3,hdfs -t tiledb:release

FROM tiledb:base

# Optional components to enable (defaults to empty).
ARG enable
# Release version number of TileDB to install.
ARG version=1.2.0
# Release version number of TileDB-Py to install.
ARG pyversion=0.1.0

# Install TileDB
RUN wget -P /home/tiledb https://github.com/TileDB-Inc/TileDB/archive/${version}.tar.gz \
    && tar xzf /home/tiledb/${version}.tar.gz -C /home/tiledb \
    && rm /home/tiledb/${version}.tar.gz \
    && cd /home/tiledb/TileDB-${version} \
    && mkdir build \
    && cd build \
    && ../scripts/install-deps.sh --enable=${enable} \
    && ../bootstrap --prefix=/usr/local --enable=${enable} \
    && make \
    && make examples \
    && make install

# Install Python bindings
RUN wget -P /home/tiledb https://github.com/TileDB-Inc/TileDB-Py/archive/${pyversion}.tar.gz \
    && tar xzf /home/tiledb/${pyversion}.tar.gz -C /home/tiledb \
    && rm /home/tiledb/${pyversion}.tar.gz \
    && wget -P /tmp https://bootstrap.pypa.io/get-pip.py \
    && python3 /tmp/get-pip.py \
    && rm /tmp/get-pip.py \
    && cd /home/tiledb/TileDB-Py-${pyversion} \
    && pip install -r requirements_dev.txt \
    && python3 setup.py install

RUN chown -R tiledb: /home/tiledb

ENV LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
EXPOSE 22

USER tiledb
WORKDIR /home/tiledb
ENTRYPOINT /bin/bash

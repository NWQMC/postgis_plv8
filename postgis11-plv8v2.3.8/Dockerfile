# see https://github.com/IonxSolutions/docker-images/blob/master/postgres-plv8/11.1/Dockerfile
# Docker image for PostgreSQL with the plv8 extension installed
# Note: building plv8 times out on Docker Hub, so instead we download pre-built binaries
# Provided by Ionx Solutions: https://www.ionxsolutions.com

# Begin by downloading binaries built on Debian stretch-slim
FROM debian:stretch-slim AS staging

ENV PLV8_VERSION 2.3.8
ENV PLV8_SHASUM="60d7ba9f214ddd4d53c9ec9d004de90bd980aedfec1833cd1ff7286cc4ef796a plv8-${PLV8_VERSION}.7z"

RUN apt-get update
RUN apt-get install -y --no-install-recommends curl ca-certificates p7zip
RUN mkdir -p /tmp/plv8
RUN curl -o /tmp/plv8/plv8-${PLV8_VERSION}.7z -SL "https://www.ionxsolutions.com/files/plv8/${PLV8_VERSION}"
RUN cd /tmp/plv8 \
    && echo ${PLV8_SHASUM} | sha256sum -c \
    && 7zr x plv8-${PLV8_VERSION}.7z

# Copy the plv8 build output into an image based on postgres

FROM mdillon/postgis:11

LABEL maintainer="[gs-w_eto_eb_federal_employees@usgs.gov](mailto:gs-w_eto_eb_federal_employees@usgs.gov)"

ENV PLV8_VERSION 2.3.8

COPY --from=staging /tmp/plv8/plv8-${PLV8_VERSION}.so /usr/lib/postgresql/${PG_MAJOR}/lib/plv8-${PLV8_VERSION}.so
COPY --from=staging /tmp/plv8/plv8.control /tmp/plv8/plv8--${PLV8_VERSION}.sql /usr/share/postgresql/${PG_MAJOR}/extension/

RUN apt-get update \
    && apt-get install -y --no-install-recommends libc++1 \
    && rm -rf /var/lib/apt/lists/* \
    && chmod 0755 /usr/lib/postgresql/${PG_MAJOR}/lib/plv8-${PLV8_VERSION}.so \
    && chmod 0644 /usr/share/postgresql/${PG_MAJOR}/extension/plv8.control \
    && chmod 0644 /usr/share/postgresql/${PG_MAJOR}/extension/plv8--${PLV8_VERSION}.sql

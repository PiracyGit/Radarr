FROM ghcr.io/linuxserver/baseimage-ubuntu:jammy

# set version label
ARG BUILD_DATE
ARG VERSION
ARG RADARR_VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="eliasbenb"

# set environment variables
ARG DEBIAN_FRONTEND="noninteractive"
ENV XDG_CONFIG_HOME="/config/xdg"
ENV RADARR_BRANCH="develop"

RUN \
  echo "**** install packages ****" && \
  apt-get update && \
  apt-get install -y --no-install-recommends \
    mediainfo \
    libicu70 && \
  echo "**** install radarr ****" && \
  mkdir -p /app/radarr/bin && \
  curl -o \
    /tmp/radarr.tar.gz -L \
    "https://github.com/PiracyGit/Radarr/releases/download/v${RADARR_VERSION}/Radarr.${RADARR_BRANCH}.${RADARR_VERSION}.linux-core-x64.tar.gz" && \
  tar xzf \
    /tmp/radarr.tar.gz -C \
    /app/radarr/bin --strip-components=1 && \
  chmod -R 775 /app/radarr/bin && \
  echo -e "UpdateMethod=docker\nBranch=${RADARR_BRANCH}\nPackageVersion=${VERSION}\nPackageAuthor=[linuxserver.io](https://linuxserver.io)" > /app/radarr/package_info && \
  echo "**** cleanup ****" && \
  apt-get clean && \
  rm -rf \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/*

# add local files
COPY root/ /

# ports and volumes
EXPOSE 7878
VOLUME /config

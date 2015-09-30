FROM phusion/baseimage:0.9.16
MAINTAINER nick.humrich@canopytax.com

# Ensure UTF-8
RUN locale-gen en_US.UTF-8
ENV LANG       en_US.UTF-8
ENV LC_ALL     en_US.UTF-8

# gpg keys listed at https://github.com/nodejs/io.js
RUN gpg --keyserver pool.sks-keyservers.net --recv-keys \
  9554F04D7259F04124DE6B476D5A82AC7E37093B \
  DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
  FD3A5288F042B6850C66B31F09FE44734EB7990E \
  94AE36675C464D64BAFA68DD7434390BDBE9B9C5

# Make sure node, npm, and python are all installed.
RUN apt-get update
RUN apt-get install curl build-essential libssl-dev libfontconfig1 python git git-core -y

# SET the version below to change node version
ENV NODEJS_VERSION 4.0.0

RUN apt-get install libfontconfig1 -fy
RUN curl -SLO "https://nodejs.org/download/release/v$NODEJS_VERSION/node-v$NODEJS_VERSION-linux-x64.tar.gz"
RUN curl -SLO "https://nodejs.org/download/release/v$NODEJS_VERSION/SHASUMS256.txt.asc"
RUN gpg --verify SHASUMS256.txt.asc
RUN grep " node-v$NODEJS_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc | sha256sum -c -
RUN tar -xzf "node-v$NODEJS_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1
RUN rm "node-v$NODEJS_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc

# And now we update npm
#RUN npm install -g npm

RUN mkdir -p /www
WORKDIR /www

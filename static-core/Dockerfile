FROM phusion/baseimage:0.9.16
MAINTAINER nick.humrich@canopytax.com

# Ensure UTF-8
RUN locale-gen en_US.UTF-8
ENV LANG       en_US.UTF-8
ENV LC_ALL     en_US.UTF-8

# Make sure node, npm, and python are all installed.

ENV IOJS_VERSION 2.5.0

RUN apt-get update
RUN apt-get install curl build-essential libssl-dev libfontconfig1 python git git-core -y

# Install iojs 2.5.0 through nave
RUN curl https://raw.githubusercontent.com/isaacs/nave/master/nave.sh > /tmp/nave.sh
RUN chmod +x /tmp/nave.sh
RUN /tmp/nave.sh usemain 2.5.0
